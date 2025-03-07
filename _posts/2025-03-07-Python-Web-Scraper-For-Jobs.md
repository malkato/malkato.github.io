---
categories:
- Scripting
layout: post
media_subpath: /assets/posts/2025-03-07-Python-Web-Scraper-For-Jobs
tags:
- Basic
- Python
title: Webscraping Script For Jobs - Python
---


*During my time applying for jobs, I got a headache for looking everyday any new open positions. Therefore it was good time to ask from ChatGPT to help me to build this script. Just for fun. It´s not great at all and I am not a good developer but ideas came into my mind so I decided to do it anyways.*

*This script is scraping from Duunitori.fi website for any latest positions in the IT-sector and saving the data into local JSON-file. It has a cute GUI, where you have options to manually fetch the data or set a automated scheduled fetching. The button for deleting whole data was for debugging purposes. I have some ideas for later development.*

*Scheduled timer is now each 30 minutes and needs to be changed from the code at the moment. There will be also colored output in the list for specific roles and cities. Now Trainee, Support and Jyväskylä's roles are in green, which are most interesting ones.*

![](2025-03-07-22-33.png)
![](2025-03-07-22-55.png)


````
import json
import requests
from bs4 import BeautifulSoup
from plyer import notification
import schedule
import time
import threading
import tkinter as tk
from tkinter import messagebox
import os

# Load the jobs from a file
def load_jobs():
    try:
        with open('jobs.json', 'r') as file:
            return json.load(file)
    except FileNotFoundError:
        return []

# Save the jobs to a file
def save_jobs(jobs):
    with open('jobs.json', 'w') as file:
        json.dump(jobs, file, indent=4)

# Define URLs and corresponding element selectors
urls_and_selectors = [
    {
        "url": "https://duunitori.fi/tyopaikat?haku=tieto-+ja+tietoliikennetekniikka+(ala)&order_by=date_posted",
        "title_selector": "job-box__hover gtm-search-result",  # Title selector
        "website": "https://duunitori.fi",  # Website URL
        "date_selector": "job-box__job-posted",  # Date selector
        "location_selector": "job-box__job-location"  # Date selector
    }
]

# Function to fetch jobs from a job listing website
def fetch_jobs():
    jobs = []
    for site in urls_and_selectors:
        url = site['url']
        title_selector = site['title_selector']
        website = site['website']
        date_selector = site['date_selector']
        location = site['location_selector']
        
        # Fetch the HTML content
        response = requests.get(url)

        # Check if the request was successful
        if response.status_code != 200:
            print(f"Failed to retrieve {url}. Status code: {response.status_code}")
            continue

        soup = BeautifulSoup(response.text, 'html.parser')

        # Find all job containers
        job_containers = soup.find_all('div', class_='grid grid--middle job-box job-box--lg')  # Adjust this to your website's structure

        if not job_containers:
            print(f"No jobs found on {url}. Check the CSS selector.")
        
        for container in job_containers:
            # Extract the job title using the title selector
            job_title_elem = container.find('a', class_=title_selector)
            if job_title_elem:
                job_title = job_title_elem.text.strip()
            else:
                continue
            job_title_elem = container.find('span', class_=location)
            if job_title_elem:
                job_location = job_title_elem.text.strip()
            else:
                continue

            # Extract the job date using the date selector
            job_date_elem = container.find('span', class_=date_selector)
            if job_date_elem:
                job_date = job_date_elem.text.strip()
            else:
                job_date = "N/A"

            jobs.append((job_title, job_date, website, job_location))
    
    return jobs

# Function to send Windows notification
def send_notification(subject, body):
    notification.notify(
        title=subject,
        message=body,
        timeout=10
    )

# Function to check for new jobs and notify
def check_for_new_jobs(app):
    print("Checking for new jobs...")
    new_jobs = fetch_jobs()
    saved_jobs = load_jobs()
    new_jobs_to_notify = []

    for job in new_jobs:
        if job not in saved_jobs:  # Check for duplicates
            saved_jobs.append(job)
            new_jobs_to_notify.append(job)

    if new_jobs_to_notify:
        subject = "New Job Opportunities Found"
        body = "\n".join([f"{job[0]}: {job[1]}" for job in new_jobs_to_notify])
        send_notification(subject, body)
        save_jobs(saved_jobs)  # Save updated job list
        print(f"Notification sent. Found {len(new_jobs_to_notify)} new job(s).")
        app.update_job_listbox(saved_jobs)  # Update the GUI with new jobs
        app.update_message_label("")  # Clear the "No new jobs" message
    else:
        print("No new jobs found.")
        app.update_message_label("No new jobs found.")  # Show message on the GUI

# Function to start the scheduler
def start_scheduler(app):
    print("Scheduler started.")
    schedule.every(30).minutes.do(lambda: check_for_new_jobs(app))

    while True:
        schedule.run_pending()
        time.sleep(1)

def start_thread(app):
    scheduler_thread = threading.Thread(target=start_scheduler, args=(app,), daemon=True)
    scheduler_thread.start()

# GUI Setup
class JobFetcherApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Job Fetcher")

        # Job Listbox to display fetched jobs
        self.job_listbox = tk.Listbox(root, width=150, height=15)
        self.job_listbox.pack(pady=20)

        # Message Label to show "No new jobs" message
        self.message_label = tk.Label(root, text="", fg="red")
        self.message_label.pack(pady=10)

        # Manual fetch button
        self.fetch_button = tk.Button(root, text="Fetch Jobs Manually", command=self.fetch_jobs_manually)
        self.fetch_button.pack(pady=10)

        # Start/Stop Scheduler Button
        self.scheduler_button = tk.Button(root, text="Start Automatic Fetch", command=self.toggle_scheduler)
        self.scheduler_button.pack(pady=10)

        # Erase Database Button
        self.erase_button = tk.Button(root, text="Erase Database", command=self.erase_database)
        self.erase_button.pack(pady=10)

        self.scheduler_running = False

        # Load initial jobs
        self.load_jobs()

    def load_jobs(self):
        self.jobs = load_jobs()
        self.update_job_listbox(self.jobs)

    def update_job_listbox(self, jobs):
        self.job_listbox.delete(0, tk.END)
        
        for job in jobs:
            job_title, job_date, website, job_location = job
            
            # Customize the color based on job title or location
            job_text = f"{job_title} - {job_date} - {website} - {job_location}"
            
            if "Helsinki" in job_location:
                self.job_listbox.insert(tk.END, job_text)
                self.job_listbox.itemconfig(tk.END, {'bg': 'yellow'})  # Highlight Helsinki jobs in yellow
            elif "Espoo" in job_location:
                self.job_listbox.insert(tk.END, job_text)
                self.job_listbox.itemconfig(tk.END, {'bg': 'yellow'})   # Highlight Espoo jobs in yellow
            elif "Support" in job_title:
                self.job_listbox.insert(tk.END, job_text)
                self.job_listbox.itemconfig(tk.END, {'bg': 'green'})    # Highlight support jobs in green
            elif "Trainee" in job_title:
                self.job_listbox.insert(tk.END, job_text)
                self.job_listbox.itemconfig(tk.END, {'bg': 'green'})  # Highlight trainee jobs in green
            elif "Jyväskylä" in job_location:
                self.job_listbox.insert(tk.END, job_text)
                self.job_listbox.itemconfig(tk.END, {'bg': 'green'})  # Highlight jobs in Jyväskylä in green
            else:
                self.job_listbox.insert(tk.END, job_text)  # Default color (no highlight)

    def update_message_label(self, message):
        self.message_label.config(text=message)

    def fetch_jobs_manually(self):
        new_jobs = fetch_jobs()
        saved_jobs = load_jobs()
        for job in new_jobs:
            if job not in saved_jobs:
                saved_jobs.append(job)
        save_jobs(saved_jobs)
        self.jobs = saved_jobs
        self.update_job_listbox(self.jobs)
        messagebox.showinfo("Jobs Fetched", f"Fetched {len(new_jobs)} new job(s).")

    def toggle_scheduler(self):
        if self.scheduler_running:
            self.scheduler_running = False
            self.scheduler_button.config(text="Start Automatic Fetch")
            print("Scheduler Stopped.")
        else:
            self.scheduler_running = True
            self.scheduler_button.config(text="Stop Automatic Fetch")
            start_thread(self)

    def erase_database(self):
        try:
            os.remove('jobs.json')  # Delete the jobs.json file
            self.jobs = []  # Clear the job list in the app
            self.update_job_listbox(self.jobs)  # Update the listbox to reflect the cleared database
            messagebox.showinfo("Database Cleared", "The job database has been cleared.")
        except FileNotFoundError:
            messagebox.showwarning("Error", "The job database file does not exist.")

# Run the app
if __name__ == "__main__":
    root = tk.Tk()
    app = JobFetcherApp(root)
    root.mainloop()

````





