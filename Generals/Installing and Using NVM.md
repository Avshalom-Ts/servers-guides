# Installing and Using NVM (Node Version Manager) [Guid](https://www.linode.com/docs/guides/how-to-install-use-node-version-manager-nvm/)

1. Install the nvm package

    ```bash
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
    ```

2. Source the package installed

    ```bash
    source ~/.bashrc
    ```

    Alternatively, you can execute the new instructions in the same console to apply them immediately:

    ```bash
    export NVM_DIR="$HOME/.nvm"
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
    [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
    ```

3. Confirm that NVM is successfully installed:

    ```bash
    command -v nvm
    ```

    Confirm the version of NVM that is running with the following command:

    ```bash
    nvm --version
    ```

4. To install the latest version of Node, run the following:

    ```bash
    nvm install node
    ```

    To install a specific version of Node, specify the major or minor release number. You can preview a list of all available Node versions with the ls-remote command:

    ```bash
    nvm ls-remote
    ```

    Install any additional versions of Node you want to use. You can specify either a major or minor release of Node to install.

    ```bash
    nvm install 17.0.1 # Specific minor release
    nvm install 19 # Specify major release only
    ```

    Review all installed versions of Node with the ls command:

    ```bash
    nvm ls
    ```

    Specify the version number of Node (major or minor release):

    ```bash
    nvm use 17
    ```

    You can also confirm the version of Node currently in use with the -v flag:

    ```bash
    node -v
    ```