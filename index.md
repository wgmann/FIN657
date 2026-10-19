#### [William Mann](https://wgmann.com), Goizueta Business School, Emory University

## Overview

This is a landing page and documentation file for the [course Github repository](https://github.com/wgmann/FIN657) that I use when I teach FIN657, which contains the slides and Python code that I use.  **Students in this course are required to run the code** contained in this repository in order to understand course concepts and complete homework assignments.

The rest of this page explains how to set up your environment and run the code. You have two options: run the code on the web through GitHub Codespaces (easier), or locally on your own computer (more advanced). Either way, you will first need to get access to the FRED and WRDS databases.

## Database credentials

You will need to set up access to two databases, FRED and WRDS.

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
This is technically not a database, but rather a front end for a large collection of common databases in finance, maintained by the Wharton School.
We will use it to download data on prices and returns for many different investments.
This system is not free to the public, but you can sign up for free access as long as you are enrolled at Emory.
Contact me if you do not already have your access set up, and I will put you in touch with the person who handles this on campus.

Once you have your FRED API key and WRDS username, proceed to either of the next two options.

## Option 1: Run notebooks on GitHub Codespaces (easier)

GitHub Codespaces provides a cloud-based environment that automatically sets up everything you need, including VSCode and an AI agent assistant. This is the easiest way to get started if you are new to these topics.

(You will need to create a free GitHub account if you don't already have one, but this is strongly recommended anyway. If you really don't want to create a GitHub account, see the next section and download the repo as a ZIP file.)

1. Navigate to the [course Github repository](https://github.com/wgmann/FIN657)
2. Click the green "Code" button. 
3. To the right of the word "Codespaces", click the three dots, then "New with options".
4. In the screen that appears, fill in your WRDS username and FRED API key in the appropriate locations. Then launch the codespace.
5. The first time you launch the codespace, you will need to wait 2-3 minutes for it to build.

At this point you can navigate to any of the notebooks we will use in the course, and run any cell by clicking on it and hitting Shift+Enter. You can also talk to the AI agent for help understanding things, changing the code, or start an entirely new analysis. You are strongly encouraged to use this when working on your homework! After you save a file, you can download it to your own computer.

If you ever close the browser, you can open the codespace back up from the same menu where you launched it in the first place. You won't have to wait for the initial setup, it will just load.

The first time you connect to WRDS from within your codespace, it will prompt you for a password (and you will need to check your phone for 2FA as well). Then it will give you the option of saving a "pgpass" file so that you don't have to keep entering your password.

You do not need to fork the repo in order to launch a codespace. But if you commit any changes, at that point Github will create a fork within your personal account. (If you don't know what these terms mean, don't worry about it.)

## Option 2: Run notebooks locally (more advanced)

Your own computer can easily run all the code that we use in class if you install a few components that it will look for. This takes a bit more work, but gives a much better experience after that.

1. Clone or download the repo to your own computer:  
    ```  
    git clone https://github.com/wgmann/FIN657  
    cd FIN657
    ```  
    If you don't have a GitHub account and don't want to create one, then download the repo as a zip folder from this site, extract it, and navigate into the directory that is created.
2. Create a virtual environment for the course and activate that environment. If you have conda, then the easiest approach is:
    ```  
    conda env create -f environment.yml
    conda activate FIN657  
    ```  
    If you don't have conda or don't want to use it, create and activate a virtual environment called `FIN657` (the commands for this depend on what OS you are using), and then 
    ```
    python -m pip install -r requirements.txt
    ```
3. Register the environment with Jupyter: With the virtual environment still activated from the previous step,  
    ```  
    python -m ipykernel install --user --name FIN657 --display-name "FIN657"  
    ```  
4. Put your database credentials in a file called `.env`:  
    First copy the provided template file `.env.example` to a new one named `.env` (note the `.` at the start of the filename!)
    ```  
    cp .env.example .env  
    ```
    Then open the file `.env` that you just created. You will see the following lines:  
    ```  
    FRED_API_KEY=YOUR_FRED_API_KEY  
    WRDS_USERNAME=YOUR_WRDS_USERNAME  
    ```
    Replace everything after the = with your info, and do not use quote marks. For example,  
    ```  
    FRED_API_KEY=abc123  
    WRDS_USERNAME=johndoe  
    ```  
    The `load_dotenv()` function that appears at the start of every notebook will load this information automatically.  
5. Open and run notebooks (`.ipynb` files): If using VSCode or another IDE, then just open the repo directory you created, select the FIN657 kernel if necessary, and navigate to any file to open and run it.  
There are also other ways to open and run the files, including directly in a web browser, but the process is a bit clunky and not worth covering for our course.

## Licensing

This repository contains instructional materials and a small amount of
supporting software code.

- **Instructional materials**, including Jupyter notebooks (`.ipynb`),
  written explanations, examples, and course content, are licensed under
  the Creative Commons Attribution–NonCommercial 4.0 International License.
- **Standalone software code files** (e.g. `.py` scripts intended for reuse
  outside the instructional context) are licensed under the MIT License.

See `LICENSE` and `LICENSE-CODE` for details.


