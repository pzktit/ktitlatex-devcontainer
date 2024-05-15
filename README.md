# ktitlatex-devcontainer

Example of configuring Dev Containers for LaTeX document preparation in Visual Studio Code.

## What you need to use it

1. [Visual Studio Code](https://code.visualstudio.com/) with `Remote Development` extension.
2. [Docker Engine](https://docs.docker.com/engine/install/) (only Linux) or [Docker Desktop](https://www.docker.com/products/docker-desktop/) (Linux, Windows, Mac).


## For the impatient

[ktitlatex | Example of using Dev Container technology for LaTeX document preparation.](https://pzktit.github.io/ktitlatex/)


## User's manual

You will find below how to configure your custom environment.


1. Clone repository to you disk

    ```bash
    git clone https://github.com/pzktit/ktitlatex-devcontainer
    ```

2. Copy folders `.vscode/` and `.devconatiner/` to the folder with your document. Then open this folder in _Visual Studio Code_ and agree on opening in container. That starts download of _TeXLive_ docker image and its adaptation to Development Container requirements.

    For the first time it is a time-consuming process. In the course of this process about 4 GB data needs to be downloaded. You can to this in advance (i.e. before opening folder in container) by issuing command (or doing the relevant operation in _Docker Desktop_)

    ```bash
    docker pull texlive/texlive
    ```

3. When the container is ready, you can open LaTeX document. This starts configuration of _LTeX_ - a multilanguage spell and grammar checker. Afterwards, your environment is ready to use. The subsequent opening of the document are quite fast as the already configured container is used.

4. Document can be compiled with `pdflatex`, `lualatex`, or `xelatex`. The default build command is selected in `.vscode/settings.json`. The compilation is triggered by pressing the `Play` icon in the top toolbar or by saving the file with updated contents.

    The `LaTeX Workshop` extension is the workhorse of the environment. Please read about its capabilities to take advantage of _Visual Studio Code_ editing fully. The language used by the _LTeX_ checking system can be set `.vscode/settings.json`. 

5. The way the container is prepared can be modified in `.devcontainer/devcontainer.json`.

### Drawbacks

When you open the another folder the container preparation process is repeated. Fortunately, the _TeXLive_ docker image from the cache will be used. Unfortunately, the _Visual Studio Code_ extensions are installed again. This is not a big harm as long as you are not using _LTeX_. In the process of bootstrapping, the extension downloads about 300 MB of data and this data is replicated in each container, so every document would additionally consume that amount of your disk space. 

### Solution

It would be much better to have a docker image of the preconfigured environment with all extensions installed and configured. The use of containers instantiated on a basis of such image would not induce additional costs per opened document or folder. The described below procedure creates such an image. 
You need an account on [Docker Hub Container Image Library](https://hub.docker.com/) or [GitHub](https://github.com/) to repeat the steps below yourself.

1.  Create container according the above description and exit _Visual Studio Code_.

1.  Find it's ID with
    ```bash
    docker ps -a | grep ktitlatex
    ```
1.  Store container as docker image in your local repository
    ```bash
    docker commit <id_here> ktitlatex
    ```
1.  Instead of the provided `devcontainer.json` use the following one
    ```json
    // devcontainer.json
    {
        "image" : "ktitlatex"
    }
    ```

You can also publish your environment.
You need an account on [Docker Hub Container Image Library](https://hub.docker.com/) or [GitHub](https://github.com/) for that.
On docker you also need to create repository `ktitlatex`.
1.  Login in your repository with
    ```bash
    docker login <your_account>/ktitlatex
    docker login ghcr.io -u <your_gh_account> -p <your_personal_access_token>
    ```
    Please use `Personal Access Tokens` for that. They can be generated via the web interface of your account.
1. Tag your local image according to the account you are using
   ```bash
   docker tag ktitlatex <your_account>/ktitlatex
   docker tag ktitlatex ghcr.io/<your_gh_account>/ktitlatex
   ```
1. Then push your image to the remote repo
    ```bash
    docker push <your_account>/ktitlatex
    docker push ghcr.io/<your_gh_account>/ktitlatex
    ```
1. Then on any system you can use the following `devcontainer.json`
    ```json
    {
        "image" : "<your_account>/ktitlatex"
        // "image" : "ghcr.io/<your_gh_account>/ktitlatex"
    }
    ```
