#### [William Mann](https://wgmann.github.io), Goizueta Business School, Emory University

## Overview

This is a landing page and documentation file for the [course Github repository](https://github.com/wgmann/FIN657) that I use when I teach FIN657.
Eventually, it may become the main course page.
For now, if you're looking for information about the course, including topics, schedule, assignments, and grading, please see the [syllabus](https://wgmann.github.io/FIN657/syllabus.pdf), or contact me directly.

This repository contains the files used when I teach the course.
It is a collection of slides, Python notebooks, and other materials.
**Students in this course are required to run the code** contained in this repository in order to understand course concepts and complete homework assignments.

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

GitHub Codespaces provides a cloud-based development environment that automatically sets up everything you need to run our course notebooks. This is the recommended approach for getting started.

(You will need to create a free GitHub account if you don't already have one, but this is strongly recommended anyway. If you really don't want to create a GitHub account, see the next section and download the repo as a ZIP file.)

1. Open this repository on GitHub in your browser.
2. Click the green "Code" button, select "Codespaces", and click "Create codespace on main" to create your own codespace for this repository.
3. The system will prompt you to enter the WRDS_USERNAME and FRED_API_KEY that you obtained above.
4. The first time you launch the codespace, you will need to wait 10-15 minutes for the environment to build.
5. Then your browser will open a web version of VSCode, and you can navigate to any of our notebooks and run the code.

For those who already use VSCode locally, you can also connect to your codespace from there instead of the browser: open the Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`), type "Connect to Codespace", and select the codespace you just created. 
This is a middle ground between a completely browser-based approach (above) and a completely local approach (below).

**Note about other cloud systems:** Github Codespaces is our preferred cloud-based system, but there are many popular alternatives with similar functionality (e.g. Binder or Colab). If you use any such system, please be careful at all times with your database credentials. Only enter them into an appropriate secrets-handling system, not into the text of the notebook file itself. On some of these systems (including Binder) there is no secrets handler at all, in which case you should not use that system to run any code from this class that connects to a database.

## Option 2: Run notebooks locally (advanced)

Your own computer can easily run all the code that we use in class if you install a few components that it will look for. This takes a bit more work, but gives a much better experience after that.

1. Clone or download the repo to your own computer:  
    ```  
    git clone https://github.com/wgmann/FIN657  
    cd FIN657
    ```  
    If you don't have a GitHub account and don't want to create one, then download the repo as a zip folder from this site, extract it, and navigate into the directory that is created.
2. Create a virtual environment for the course and activate that environment. The recommended approach is with Conda:
    ```  
    conda env create -f environment.yml
    conda activate FIN657  
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
5. Open and run notebooks (`.ipynb` files): If using VSCode, then just open the repo directory you created, select the FIN657 kernel (if this does not happen automatically), and navigate to any file to open and run it.  
    You can also avoid using VSCode, and get a slightly better visual experience, by opening the notebooks directly in a browser window. However, the commands for this are a bit clunky:
    - Make sure your terminal window is running from inside the directory you created earlier, with the virtual environment activated.
    - Enter `jupyter notebook` to launch a browser window with a view of the course files. If it does not happen automatically, look for a URL in the output of the command, copy and paste it into a browser window.
    - Navigate to any file to open and run it. You may need to select the `FIN657` kernel if this does not happen automatically.
    - When you are done working, go back to the window where you entered `jupyter notebook`, and enter `Ctrl+C` to kill it.


## Licensing

This repository contains instructional materials and a small amount of
supporting software code.

- **Instructional materials**, including Jupyter notebooks (`.ipynb`),
  written explanations, examples, and course content, are licensed under
  the Creative Commons Attribution–NonCommercial 4.0 International License.
- **Standalone software code files** (e.g. `.py` scripts intended for reuse
  outside the instructional context) are licensed under the MIT License.

See `LICENSE` and `LICENSE-CODE` for details.


