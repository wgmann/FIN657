This repository contains course materials for FIN 657 taught by William Mann at Emory University. 
See the [landing page](https://wgmann.github.io/FIN657) for more information.
To use this repo:

    git clone https://github.com/wgmann/FIN657
    cd FIN657
    conda env create -f environment.yml
    conda activate FIN657
    python -m ipykernel install --user --name FIN657 --display-name "FIN657 (conda)"
    cp .env.example .env

...add WRDS username and FRED API to .env, then

    jupyter notebook

...and navigate to any notebook file to run it.
