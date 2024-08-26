# Virtual environment and package management

Managing Python projects efficiently involves isolating dependencies and ensuring that each project has the right packages and versions it needs. This guide will walk you through creating and managing virtual environments, using `pip` and `conda` for package management, and handling Jupyter kernels.

## Understanding package managers: pip vs Conda
Python has multiple package managers, with `pip` and `conda` being the most popular.
- **pip**: The default package installer for Python. It installs packages from the Python Package Index (PyPI). `pip` is widely used and integrates seamlessly with virtual environments created by `venv` or `virtualenv`.
- **Conda**: An open-source package management system and environment management system that can handle both Python and non-Python packages. It's part of the Anaconda distribution.


## Virtual environments

### Creating virtual environments
Virtual environments allow us to create isolated spaces for different projects, ensuring that each project has its own dependencies.

#### Using `python -m venv`
The `venv` module is included in Python 3.3 and later. It allows us to create lightweight virtual environments.
1. **Create a virtual environment**
   ```bash
   python -m venv my_env
   ```

   This creates a directory named `my_env` containing the virtual environment.
2. **Activate the virtual environment**
     ```bash
     my_env\Scripts\activate
     ```

#### Using Conda
Conda is both a package and environment manager. It is especially useful for managing dependencies in data science projects.
1. **Create a Conda environment**
   ```bash
   conda create -n my_conda_env python=3.9
   ```

   This creates a new environment named `my_conda_env` with Python 3.9.
2. **Activate the Conda environment**
   ```bash
   conda activate my_conda_env
   ```

### Activating and deactivating virtual environments

#### With `venv`
- **Activate:**
    ```bash
    my_env\Scripts\activate
    ```
- **Deactivate:**
    ```bash
    deactivate
    ```

#### With Conda
- **Activate:**
  ```bash
  conda activate my_conda_env
  ```
- **Deactivate:**
  ```bash
  conda deactivate
  ```

### Deleting a virtual environment

#### Using `venv`
1. **Deactivate the environment:**
   ```bash
   deactivate
   ```
2. **Delete the environment:** Simply delete the directory containing the virtual environment.
    ```bash
    rmdir /s /q my_env
    ```

#### Using Conda
1. **Deactivate the environment:**
   ```bash
   conda deactivate
   ```
2. **Remove the environment:**
   ```bash
   conda remove -n my_conda_env --all
   ```

---

## Packages

### Installing packages
Once our virtual environment is activated, we can install packages using `pip` or `conda`.

#### Using pip
`pip` installs packages from PyPI.
  ```bash
  pip install package_name
  ```

- **Use `pip` when:**
  - We are within a `venv`-based virtual environment.
  - The package is available on PyPI and doesn't have complex dependencies.
  - We need the latest version of a package that might not yet be available through Conda.

#### Using Conda
`conda` installs packages from the Anaconda repositories.
  ```bash
  conda install package_name
  ```

- **Use `conda` when:**
  - We are working within a Conda environment.
  - We need to install packages that have complex dependencies, especially those requiring compilation or with non-Python dependencies (e.g., `numpy`, `pandas`, `scikit-learn`, `tensorflow`).
  - We are working on data science or machine learning projects where Conda's package ecosystem is advantageous.


**Note:** It's generally recommended to prefer `conda` for package management within Conda environments and `pip` within `venv` environments. Mixing `pip` and `conda` within the same environment can sometimes lead to dependency conflicts. A package installed with Conda is not automatically available to pip. However, if the same package is installed using pip, it will coexist, potentially leading to conflicts. This is why it's recommended to use one package manager per environment.


### Uninstalling a package

#### Using pip
```bash
pip uninstall package_name
```

#### Using Conda
```bash
conda remove package_name
```

**Note:** When we uninstall a package with pip, it only removes the package installed by pip. It does not affect any packages installed by conda. Also, when we uninstall a package with conda, it removes the package from the Conda environment. It does not remove packages installed with pip.

---

## Managing virtual environment, kernels and widgets in Jupyter notebook
When working with Jupyter notebooks, it’s essential to manage different environments and their dependencies effectively. This ensures that our notebooks use the correct versions of libraries and tools. And Jupyter kernels allow these environments to be used seamlessly within Jupyter Notebook or Jupyter Lab.

Therefore, we might want to use our virtual environments as separate kernels. Jupyter kernels are the underlying processes that run the code in our notebooks. By registering virtual environment as a Jupyter kernel, we can ensure that a specific notebook runs with the correct environment, avoiding conflicts with other projects. Additionally, certain packages like `ipywidgets` enhance the interactivity of Jupyter notebooks.

### Installing Jupyter kernel

1. **Create a new virtual environment:** Before we can use a virtual environment in Jupyter, we need to create and activate it. Conda is recommended for creating and managing virtual environments, especially in data science and ML projects. Conda can handle complex dependencies and non-Python packages better than pip, making it a more robust choice for setting up environments that we intend to use in Jupyter notebooks.
   ```bash
   conda create --name my_env python=3.9.13
   ```

   This command creates a new Conda environment named `my_env` with Python 3.9.13.

2. **Activate the tnvironment:**
   ```bash
   conda activate my_env
   ```

   Activating the environment ensures that all subsequent commands run within this environment.

3.  **Installing Jupyter kernel:**
    1. **Install `ipykernel`:** This package allows our virtual environment to be used as a Jupyter kernel.
      ```bash
      pip install ipykernel
      ```

    2. **Install `ipywidgets`:** This package provides interactive widgets for Jupyter notebooks.
      ```bash
      pip install ipywidgets
      ```

4. **Registering the kernel:** After installing `ipykernel`, register the environment as a Jupyter kernel. Registering the environment as a kernel allows us to select it in Jupyter notebooks or lab, ensuring that the notebook uses the correct environment.
    ```bash
    python -m ipykernel install --user --name=my_env --display-name "Python (my_env)"
    ```

    - **`--user`**: Installs the kernel for the current user.
    - **`--name`**: A unique identifier for the kernel.
    - **`--display-name`** (optional): The name that will appear in Jupyter's interface.

### Removing a Jupyter kernel
When a virtual environment is no longer needed, it’s a good practice to clean up by removing the associated Jupyter kernel and deleting the environment. If we registered a kernel using `ipykernel`, we might want to remove it when it's no longer needed.

1. **Deactivate the environment:** Before removing the kernel, deactivate the environment to ensure it’s not in use:
   ```bash
   conda deactivate
   ```
2. **List installed kernels:** To see the currently registered kernels and their paths, use:
   ```bash
   jupyter kernelspec list
   ```
3. **Remove the kernel:** Remove the kernel associated with the environment:
   ```bash
   jupyter kernelspec uninstall my_env
   ```

   Replace `my_env` with the name of the kernel we wish to remove.
4. **Listing existing virtual environments:** To list all existing Conda environments along with their paths, use:
   ```bash
   conda env list
   ```
5. **Delete the virtual environment:** After removing the Jupyter kernel, we can delete the environment entirely:
   ```bash
   conda remove --prefix /my_env --all
   ```

   `/my_env` is the path to the environment.

This process ensures that the Jupyter environment remains clean and organized, with only the necessary kernels and environments in use.