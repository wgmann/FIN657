#### [William Mann](https://wgmann.github.io), Goizueta Business School, Emory University

---

## Overview

This is a landing page and documentation file for the [course Github repository](https://github.com/wgmann/FIN657) that I use when I teach FIN657.
Eventually, it may become the main course page.
For now, if you're looking for information about the course, including topics, schedule, assignments, and grading, please see the [syllabus](https://wgmann.github.io/FIN657/syllabus.pdf), or contact me directly.

This repository contains the files used when I teach the course.
It is a collection of slides, Python notebooks, and other materials.
**Students in this course are required to run this code locally** in order to understand the course concepts and complete the homework assignments.

There are several ways to get the code running locally.
The easiest way is to clone the course repo, and set up a Conda environment as specified by the included `environment.yml` file.
The rest of this page explains the steps behind this.

---

To run the code on your own computer, you will first need API access to two databases, FRED and WRDS.

- [FRED](https://fred.stlouisfed.org/): 
This is a public data source maintained by the Federal Reserve Bank of St Louis.
It has a wide range of macroeconomic and survey data as well as government bond yields.
You can browse the page for free and even create interactive figures. 
(For example, here's a [figure](https://fred.stlouisfed.org/graph/?g=Jf2U) I'll show a few times in class.)
To pull data over the API, you need to create an account and sign up for a free API key.
Instructions are located [here](https://fred.stlouisfed.org/docs/api/api_key.html).
The API key is a string of letters or numbers that you can include in your data request, much like a password.
The instructions below will explain how to use it.
- [WRDS](https://wrds.wharton.upenn.edu/): 
This is not really a database but rather a front end for a large collection of common databases in finance.
We will use it to download data on prices and returns for many different investments.
This database is not free to the public, but you can sign up for free access as long as you are enrolled at Emory.
Contact me if you do not already have your access set up, and I will put you in touch with the person who handles this on campus.

Then you will need to follow these steps:


1. Clone or download the repo to your own computer.
2. Create a Conda environment from `environment.yml`.
3. Create a personal `.env` file with your credentials.
4. Launch a jupyter notebook server.

Follow the steps below in order.
Some of these steps will require you enter commands into a terminal window.
On Mac, this is an app called Terminal.
On Windows, use Command Prompt or PowerShell.

---

## 1. Download or clone this repository

If you know how to use git:

    git clone https://github.com/wgmann/FIN657
    cd FIN657

If you haven't installed or used git before, but want to try, see the [official webpage](https://git-scm.com) for resources to help you get started.

If you don't want to use git, you can download the repository as a ZIP file from the GitHub website and then unzip it.

(In either case, note that I will be adding materials to the repo as the semester progresses. If you did `git clone` then you can always pull the updates with `git pull`. If you are downloading from the GitHub website then you can navigate there to find the new files, or download the whole repo again.)

All remaining commands below should be run **from inside the repository directory**.

---

## 2. Create the Conda environment

### Background

Python is a language. To use it, you need to install a program called a Python interpreter. To make it useful, you typically also have to download and install many add-on packages with extra features. When sharing code, it becomes very important to make sure you have the same version of the Python interpreter, and all the same packages and their versions, as the person who wrote the code initially. As you can imagine this can get complicated very quickly. 

One modern solution is, for each project that you work on (like this teaching repo), to have a dedicated "virtual environment" that includes a Python interpreter and the necessary packages for the project to work. When you want to do any work in this project (e.g. run some of the code in the repo), you first "activate" the environment to load the correct interpreter and packages, and then you can be confident that everything will run the same on your end as on mine.

The main point of Conda is to make this process easier. 
You give it a special kind of file called a `yml` file, which specifies a version of Python you want to use, and all the Python packages you need.
It then creates an environment matching that description which you can activate whenever you want to work on the project.
Then, anyone who wants to share code with you can also bundle a `yml` file that describes the environment the code should run in.
You can use that yml to build an environment, activate it, and run the code from inside.

Conda is especially common for teaching, but there are many other similar tools out there based on the same idea.
You can think of all this as a very simple example of the idea of "infrastructure as code":
the codebase includes a description of the system on which it should run, and that system is built around it.

### Steps to follow

Install Conda if you don't already have it.
Several different versions exist.
The most common are Miniconda or Anaconda, both of which are fine.

Next, open a terminal and navigate to the repo directory `FIN657`.
You will see a file there called `environment.yml`.
Use this file to create your environment by running:

    conda env create -f environment.yml

This will create a Conda environment named `FIN657`.
Once the environment is created, activate it:

    conda activate FIN657

Finally, with the conda environment activated, register this environment with Jupyter so that it appears as a selectable kernel in notebooks:

    python -m ipykernel install --user --name FIN657 --display-name "FIN657 (conda)"

This step makes it easy to ensure your notebook is using the correct Python environment.

---

## 3. Set up your credentials (`.env` file)

### Background

The code posted in this repo frequently connects to outside databases, which requires a username and password.
When writing code for yourself, if you need to do this, you could just write out your username and perhaps even your password as part of the code.
But when collaborating on code, this is a bad idea for both security reasons (since the code will be sent over the internet) and for practical reasons (collaborators with different credentials will have to change the code before it can work on their end).

Instead, the best approach is to have each collaborator create a "configuration" file to store all information that might be different for them from others.
The goal is to have all the code look identical outside of this configuration file.
The file itself will look different for each collaborator, and in fact each collaborator should never share their file with others (to avoid accidentally leaking this confidential information).
However, to keep everyone on the same page, a standard approach is to include in the repo a template configuration file that everyone can edit to their own details.

The most common convention is that the configuration file is called `.env` (notice the filename begins with a `.` which is a common way of labeling a system or configuration file). The template to help create the file is called `.env.example`. You use this template to create your own `.env` file, and then the code will look for that file when it runs to pull your user-specific information. The steps below explain how to set up your `.env` file correctly.

### Steps to follow

In the repo, you will see a file called `.env.example`. Make a copy of this file and rename it to `.env`.

macOS / Linux:

    cp .env.example .env

Windows:

    copy .env.example .env

Next, open `.env` in any text editor, and replace the placeholder values with your own information.

The file looks like this:

    FRED_API_KEY=YOUR_FRED_API_KEY
    WRDS_USERNAME=YOUR_WRDS_USERNAME

Replace:

- `YOUR_FRED_API_KEY` with your personal FRED API key
- `YOUR_WRDS_USERNAME` with your WRDS username

Do not use quotes or spaces in the above. For example if your FRED key is abc123 and your WRDS username is jdoe, then the two lines in your file should be `FRED_API_KEY=abc123` and `WRDS_USERNAME=jdoe`.

The Python code automatically loads variables from `.env` using the `python-dotenv` package.
So as long as your `.env` file is present and correctly filled out, you should not need to personalize the code in any way to get it working.

**Note:** It is possible to store your WRDS password in .env, but you do not need to and I recommend you don't.
Instead, the wrds library will prompt you for your password the first time you connect,
then will store it in a secure .pgpass file on your computer for future logins.
This is better than having you type the password in cleartext into the .env file.

If you are prompted for your WRDS password every time you run the code, 
it usually means the `.pgpass` file was not created correctly.
This is usually easy to troubleshoot and I can help if needed.

---

## 4. Opening and running notebook files

### Background

Once you have set up the Conda environment and your credentials file, you can run any Python code and expect it to match my results.
For this class, our code examples are contained in notebook files, which require some explanation if you are not familiar.

Notebooks are files that include both blocks of code, results from that code including figures, and blocks of text discussing the code.
These files are viewed through web browsers like a webpage.
To use them, you first use the `jupyter notebook` command to start a tiny server running on your own computer that can talk to a web browser and tell it what to display.
Once that server is running, you can use a web browser to navigate and open Jupyter files.

### Steps to follow

To open, edit, and run a notebook, do the following:

(1) Open a terminal

(2) Navigate to the repository directory

(3) Activate the environment:

    conda activate FIN657

(4) Launch jupyter: 

    jupyter notebook

(5) Look for the browser window that opens up. If one does not open automatically, find the URL that appears in the output of the above command, and paste it into a browser window.

(6) Navigate to the file that you want and click once to open.

(7) When finished, you can close this file and use the earlier tab to navigate to a different one if desired.

(8) When you are completely done, close all the browser windows that have opened up, go back to the terminal from earlier, kill the notebook server by entering `Ctrl+C`, and exit the terminal window.

---

## A word about Binder

Binder is a free online environment that lets you launch and run code directly from a GitHub repository like this one.
It can be useful for previewing how our notebooks are structured and how the code is meant to work once you are set up locally.

However, Binder should be treated as a public and untrusted environment. 
It does not provide security guarantees appropriate for handling secrets.
So, as a matter of standard security practice, 
**you should never enter confidential information into Binder**,
including usernames, passwords, or API keys (and this may even violate terms of service for the databases we use). 

For this reason, any use of Binder with this repository should be limited to code that does not require downloading protected data. 
Any code that requires credentials should be run locally using a .env file as described above.
This page does not include instructions on using Binder, to avoid encouraging credential use in an environment that is not designed for it.

---

## Licensing

This repository contains instructional materials and a small amount of
supporting software code.

- **Instructional materials**, including Jupyter notebooks (`.ipynb`),
  written explanations, examples, and course content, are licensed under
  the Creative Commons Attribution–NonCommercial 4.0 International License.
- **Standalone software code files** (e.g. `.py` scripts intended for reuse
  outside the instructional context) are licensed under the MIT License.

See `LICENSE` and `LICENSE-CODE` for details.

