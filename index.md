#### [William Mann](https://wgmann.github.io), Goizueta Business School, Emory University

---

## Overview

This is a landing page and documentation file for the [course Github repository](https://github.com/wgmann/FIN657) that I use when I teach FIN657.
Eventually, it may become the main course page.
For now, if you're looking for information about the course, including topics, schedule, assignments, and grading, please see the [syllabus](https://wgmann.github.io/FIN657/syllabus.pdf), or contact me directly.

This repository contains the files used when I teach the course.
It is a collection of slides, Python notebooks, and other materials.
**Students in this course are required to run this code** in order to understand the course concepts and complete the homework assignments.

There are several ways to do this.
The easiest way to get started is to use Google Colab.
A more effective way, but slightly more work, is to clone the course repo and run the code locally
The rest of this page explains both approaches.

## Databases

Regardless of which approach you take, you will first need API access to two databases, FRED and WRDS.

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
This system is not free to the public, but you can sign up for free access as long as you are enrolled at Emory.
Contact me if you do not already have your access set up, and I will put you in touch with the person who handles this on campus.

## Google Colab

oogle Colab is a free service that offers you a way to run files like the code examples for our class in a browser window using Google's resources, with no need to install or set up anything on your own computer. The downside is that you only get limited resources, there is a time limit, the system can slow down if there's heavy traffic, and everything you do gets deleted when you close the window. For our course, this is an easy way to tinker with code but not a great environment if you will be spending significant time on the code.

1. Open any of our `.ipynb` files on Google Colab.
    - I will try to provide a link for this in each case.
    But in case I forget, you can also just open the Google Colab website, select "Github", search for my username `wgmann`, select this course `FIN657`, and scroll to the file you want.
    - Or if you see the link to the file you want on Github.com, you can just replace the first part of the URL with `https://colab.research.google.com/github/`
2. Click on the "Secrets" tab to the left. Add a secret called `FRED_API_KEY`, and another called `WRDS_USERNAME`, with your personal values of these (see above).
    - When you run the code, and the system tries to connect to WRDS, it will prompt you for your password at that point.
    - *Do not* type your personal information into the notebook file itself as this is less secure than the "Secrets" tab.
3. Now just hit "Run all" in the notebook.
    - Or you can execute individual cells one at a time, but make sure you start with the first code cell which is labeled "Standard setup cell".
    - Be patient while this setup cell runs: depending on traffic conditions it can take 10 to 30 seconds.

## Local setup

Your own computer can easily execute all the code that we use in class. However, you have to install a few components that it will look for. The rest of this file walks you through how to do it. The steps are:

1. Clone or download the repo to your own computer.
2. Create a virtual environment or Conda environment for the course, activate that environment, and install the course tools.
3. Create a personal `.env` file with your credentials.
4. Launch a jupyter notebook server, or open in VSCode or a similar environment.

Some of these steps will require you enter commands into a terminal window.
On Mac, this is an app called Terminal.
On Windows, use Command Prompt or PowerShell.

### 1. Download or clone this repository

If you know how to use git:

    git clone https://github.com/wgmann/FIN657
    cd FIN657

If you haven't installed or used git before, but want to try, see the [official webpage](https://git-scm.com) for resources to help you get started.
If you don't want to use git, you can download the repository as a ZIP file from the GitHub website and then unzip it.

(In either case, note that I will be adding materials to the repo as the semester progresses. If you did `git clone` then you can always pull the updates with `git pull`. If you are downloading from the GitHub website then you can navigate there to find the new files, or download the whole repo again.)

All remaining terminal commands should be entered from the repo folder (where you are after running the above commands).

### 2. Create a virtual environment or Conda environment

A Python virtual environment is a dedicated installation of Python along with various supporting packages. It's common to have a specific environment for every project you work on (such as this course). When you want to do any work in this project (e.g. run some of the code in the repo), you first "activate" the environment to load the correct interpreter and packages.

Conda is a tool for managing virtual environments that provides some extra functionality. It is very common in teaching and you have likely seen it before. The instructions below will focus on Conda, but you are free to set up a non-Conda virtual environment using the requirements.txt in the repo if you prefer.

Using Conda (after first installing it),
do this from the course directory where you ended up after running the earlier commands:

    conda create -n FIN657 python=3.12
    conda activate FIN657
    pip install .

At this point you have created a virtual environment that you can activate anytime you want to run code in our class.
To activate it, do `conda activate FIN657` from anywhere on your computer.

Finally, with the environment activated, register it with Jupyter so that it appears as a selectable kernel in notebook systems or VSCode:

    python -m ipykernel install --user --name FIN657 --display-name "FIN657"

### 3. Set up your credentials (`.env` file)

The code posted in this repo frequently connects to outside databases, which requires a username and password.
When writing code for yourself, if you need to do this, you could just write out your username and perhaps even your password as part of the code.
But when collaborating on code, this is a bad idea for both security reasons (since the code will be sent over the internet) and for practical reasons (collaborators with different credentials will have to change the code before it can work on their end).

Instead, the best approach is to have each collaborator create a "configuration" file to store all information that might be different for them from others.
The goal is to have all the code look identical outside of this configuration file.
The file itself will look different for each collaborator, and in fact each collaborator should never share their file with others (to avoid accidentally leaking this confidential information).
However, to keep everyone on the same page, a standard approach is to include in the repo a template configuration file that everyone can edit to their own details.

The most common convention is that the configuration file is called `.env` (notice the filename begins with a `.` which is a common way of labeling a system or configuration file). The template to help create the file is called `.env.example`. You use this template to create your own `.env` file, and then the code will look for that file when it runs to pull your user-specific information. The steps below explain how to set up your `.env` file correctly.

In the repo, you will see a file called `.env.example`. Make a copy of this file and rename it to `.env`.
You can copy with a visual tool like Explorer or Finder, or else use terminal commands: On macOS / Linux,
`cp .env.example .env`
or on Windows
`copy .env.example .env`

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

What about your WRDS password?
The code will prompt you for this the first time you connect to WRDS,
then (if you approve) it will store it in a secure .pgpass file on your computer for future logins.

### 4. Opening and running notebook files

Once you have set up the Conda environment and your credentials file, you can run any Python code and expect it to match my results.
For this class, our code examples are contained in notebook files, which are files that include both blocks of code, results from that code including figures, and blocks of text discussing the code.

The easiest way to open notebook files is through VSCode, which many of you have probably used in previous classes. (Many other IDEs also provide similar functionality.) Just open the folder where you copied our course materials and you will be able to navigate to the notebook you want in the window to the left. The first time you run the code, you will also have to select the virtual environment you created in the earlier step.

The other possible way to view the files is through web browsers like a webpage.
To do this, you first use the `jupyter notebook` command to start a tiny server running on your own computer that can talk to a web browser and tell it what to display.
Once that server is running, you can use a web browser to navigate and open Jupyter files.
This approach is a bit more complex but gives a better feel of what the notebook is "meant" to look like.

1. Open a terminal
2. Navigate to the repository directory
3. Activate the environment: `conda activate FIN657`
4. Launch jupyter: `jupyter notebook`
5. Look for the browser window that opens up. If one does not open automatically, find the URL that appears in the output of the previous command, and paste it into a browser window.
6. Navigate to the file that you want and click once to open.
7. When finished, you can close this file and use the earlier tab to navigate to a different one if desired.
8. When you are completely done, close all the browser windows that have opened up, go back to the terminal from earlier, kill the notebook server by entering `Ctrl+C`, and exit the terminal window.

### Cloud systems other than Colab

There are several systems like Colab that offer cloud-based viewing and execution of notebook files (e.g. Binder).
However, I *strongly recommend against* using any other systems.
Google Colab has a Secrets tab that can reliably store your secret database credentials in a secure way.
With other systems, you will have to type them into the code itself, and then it is much harder to be confident about their security.

## Licensing

This repository contains instructional materials and a small amount of
supporting software code.

- **Instructional materials**, including Jupyter notebooks (`.ipynb`),
  written explanations, examples, and course content, are licensed under
  the Creative Commons Attribution–NonCommercial 4.0 International License.
- **Standalone software code files** (e.g. `.py` scripts intended for reuse
  outside the instructional context) are licensed under the MIT License.

See `LICENSE` and `LICENSE-CODE` for details.

