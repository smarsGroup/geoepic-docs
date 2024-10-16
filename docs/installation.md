<!-- ## <strong>Installation</strong> -->
Before starting the setup, ensure you have [`wget`](https://cloudcone.com/docs/article/the-linux-wget-command/) and [`conda`](https://docs.conda.io/projects/conda/en/latest/user-guide/install/linux.html) installed. <br> Follow the links for corresponding installation guides.

### Steps to Set Up the GeoEPIC Toolkit

1. **Create a virtual environment in conda**
    ```bash
    conda create --name epic_env python=3.11.9
    ```
2. **Activate the environment**
    ```bash
    conda activate epic_env
    ```

3. **Install the GeoEPIC Toolkit**  <br>

    **Option 1: Install Directly from GitHub (recommended)**
        ```bash
        pip install git+https://github.com/smarsGroup/geo-epic.git
        ```
    **Option 2: Install locally (for developers)** <br>

        This option is advisable only for developers.
        ```bash
        git clone https://github.com/smarsGroup/geo-epic.git
        ```
        ```bash
        cd geo-epic
        ```
        ```bash
        pip install .
        ```

4. **Contributing to GeoEPIC ** <br>

    **Creating Issues** <br>
    If you find any bugs, have any suggestions or feature requests, please create an issue on GitHub.

    **Pull Requests** <br>
    If you want to contribute to the code base, please create a new branch and send a pull request.
    Also, contact us for any kind of collaboration.

Now, the GeoEPIC toolkit is sucessfully installed on the **epic_env** conda environment. All the commands and python API can be accessed via that conda environment. Happy coding!
