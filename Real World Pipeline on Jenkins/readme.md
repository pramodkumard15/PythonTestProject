# Real World Jenkins Pipeline

## Overview
This project demonstrates setting up a **Jenkins pipeline** for automating Python application tasks. The pipeline runs the process of setting up a Python environment, installing dependencies, running tests, and building the application using Jenkins.

---

## Tasks Completed

### 1. **Creating a Python Project in VS Code**

- Opened **VS Code** and created a folder called `Python-Code`.
- Inside the folder, created a Python script file (`example.py`).
- Added a simple Python script:
    ```python
    print("Jenkins Pipeline Example")
    ```

### 2. **Setting Up Git Repository**

- Initialized a **Git repository** in the `Python-Code` folder:
    ```bash
    git init
    ```
- Created a `.gitignore` file to exclude unnecessary files and committed the changes.
- Created a new **GitHub repository** (`https://github.com/pramodkumard15/PythonTestProject.git`) and pushed the local repository to GitHub:
    ```bash
    git remote add origin https://github.com/pramodkumard15/PythonTestProject.git
    git push -u origin main
    ```

### 3. **Creating Jenkins Pipeline**

- Opened **Jenkins** and created a new **Pipeline Job** called `PythonBuild`.
- Configured the **Pipeline** to pull the code from the GitHub repository.
- Set the **Branch** to `main`.
- Used **Pipeline script from SCM** option in Jenkins to link the GitHub repository.

### 4. **Creating a `Jenkinsfile` for Pipeline Configuration**

- Created a file named `Jenkinsfile` in the root directory of the project.
- Wrote the following pipeline code to define multiple stages: Checkout, Install Dependencies, Run Tests, and Build Application.
    ```groovy
    pipeline {
        agent any
        environment {
            PYTHON_ENV = "venv"
        }
        stages {
            stage('Checkout') {
                steps {
                    checkout scm
                }
            }
            stage('Install Dependencies') {
                steps {
                    script {
                        sh 'python3 -m venv $PYTHON_ENV'
                        sh './$PYTHON_ENV/bin/pip install -r requirements.txt'
                    }
                }
            }
            stage('Run Tests') {
                steps {
                    script {
                        sh './$PYTHON_ENV/bin/pytest tests/'
                    }
                }
            }
            stage('Build Application') {
                steps {
                    script {
                        sh './$PYTHON_ENV/bin/python setup.py install'
                    }
                }
            }
        }
        post {
            always {
                echo "Build completed."
            }
            success {
                echo "Build succeeded!"
            }
            failure {
                echo "Build failed."
            }
        }
    }
    ```

### 5. **Creating a `requirements.txt` File**

- Created a `requirements.txt` file to list the necessary dependencies, like `pytest` for running tests.
    ```txt
    pytest
    ```

### 6. **Creating a Test Folder and Adding Test Files**

- Created a **`tests/`** folder in the project root.
- Added test files, such as `test_example.py`, which includes basic unit tests using `pytest`:
    ```python
    def test_sample():
        assert 1 + 1 == 2
    ```

### 7. **Running the Jenkins Pipeline**

- Triggered the **Jenkins pipeline** to run the following:
  - **Checkout the repository**.
  - **Set up a Python virtual environment** (`venv`).
  - **Install dependencies** from `requirements.txt`.
  - **Run the tests** using `pytest`.
  - **Build the Python application** (optional, if `setup.py` exists).
  
- Observed the **Console Output** in Jenkins, which showed the status of each stage.

### 8. **Pipeline Post Actions**

- Configured **post-build actions** to display messages based on the success or failure of the pipeline:
    - **always**: Always runs after the pipeline, printing the message "Build completed."
    - **success**: Runs only if the build is successful, printing "Build succeeded!"
    - **failure**: Runs only if the build fails, printing "Build failed."

### 9. **Successful Jenkins Build**

- The Jenkins job was successfully built after following the above steps.
- Jenkins displayed the **success message** "Build succeeded!" after completing all pipeline stages.

---

## Conclusion

In this tutorial, you learned how to set up a **Jenkins pipeline** for a Python project, automating various tasks like:
- Pulling code from GitHub.
- Installing dependencies.
- Running tests.
- Building the application.

You also gained hands-on experience with Jenkinsfile creation, Python virtual environments, and integrating tests with Jenkins.

---

## Troubleshooting

- **Build Fails**: Check the **Console Output** in Jenkins to understand errors, like missing dependencies or issues with paths.
- **Missing `Jenkinsfile`**: Make sure the `Jenkinsfile` is committed to the right Git branch and is available in the GitHub repository.
- **GitHub Credentials**: If Jenkins cannot access the GitHub repository, check your credentials and permissions.

---

## Future Enhancements

- Add more tests in the `tests/` folder.
- Integrate with Slack or email for build notifications.
- Add a **Docker container** for the Python environment.
