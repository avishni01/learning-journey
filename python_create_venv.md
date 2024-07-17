# Creating and Using a Python Virtual Environment (venv) on Mac

Creating and using a Python virtual environment on a Mac is a straightforward process that helps manage project-specific dependencies separately from your global Python installation. 

## Creating a Virtual Environment

1. **Open Terminal**: You can find Terminal in your Applications folder, under Utilities, or search for it using Spotlight.

2. **Navigate to Your Project Directory**: Use the `cd` command to navigate to the directory where you want to create your virtual environment. For example:
   ```bash
   cd path/to/your/project
   ```

3. **Create the Virtual Environment**: Run the following command to create a virtual environment named `venv` (you can name it anything):
   ```bash
   python3 -m venv venv
   ```
   This command creates a directory named `venv` in your project folder, containing the virtual environment.

## Activating the Virtual Environment

1. **Activate Virtual Environment**: To start using the virtual environment, you need to activate it with the following command:
   ```bash
   source venv/bin/activate
   ```
   Once activated, your terminal prompt will change to indicate that you are now working inside the virtual environment.

## Installing Libraries in the Virtual Environment

1. **Install Libraries**: With the virtual environment activated, you can install libraries using `pip`. For example, to install Flask:
   ```bash
   pip install Flask
   ```

2. **Verify Installation**: To check the installed packages in the virtual environment, use:
   ```bash
   pip list
   ```

## Deactivating the Virtual Environment

1. **Deactivate**: When you are done working in the virtual environment, you can deactivate it by simply running:
   ```bash
   deactivate
   ```
   This will return you to the global Python environment.

## Notes

- Make sure you have Python 3 installed on your Mac. You can check this by running `python3 --version` in the Terminal.
- Each project can have its own virtual environment to manage dependencies separately.
- Remember to activate the virtual environment every time you work on the project.

This process helps to keep your global Python environment clean and your project dependencies well managed.
