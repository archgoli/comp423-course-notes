# Setting up a dev container for Go


* Primary author: [Archana Goli](https://github.com/archgoli)
* Reviewer: [Shana Tran](https://github.com/svytran)

Welcome! In this tutorial, you'll learn how to set up a basic Go development container in Visual Studio Code and how to run and compile a simple program. 

!!! note "Why Go?"
    add stuff

## Prerequisites

1. **A GitHub account**
2. **Git installed**
3. **VS Code** 
4. **Docker installed**: Required to run the dev container. 
5. **Command-line basics**: Your COMP211 command-line knowledge will serve you well here. If in doubt, review the Learn a CLI text!

## Part 1: Set up Git Repository 

### Step 1. Create a local directory and initialize Git
1. Open your terminal or command prompt 
2. Create a new directory for your project. (Note: Of course, if you'd like to organize this tutorial somewhere else on your machine, go ahead and change into that parent directory first. By default this will be in your user's home directory.): 

```bash
mkdir comp423-go-tutorial
cd comp423-go-tutorial
```
3. Initialize a new Git repository: 
```bash
git init
```
4. Create a README file: 
```bash
echo "# Go tutorial" > README.md 
git add README.md
git commit -m "Initial commit with README"
```
### Step 2. Create a Remote Repository on GitHub
1. Log in to your GitHub account and navigate to the [Create a New Repository page](https://github.com/new). 
2. Fill in the details as follows: 
    - **Repository Name**: ```comp423-go-tutorial```
    - **Description**: Tutorial of Go, a statically typed, compiled high-level general purpose programming language developed by Google.
    - **Visibility**: Public
3. Do not initialize the repository with a README, .gitignore, or license.
4. Click **Create Repository**.

### Step 3. Link your Local Repository to GitHub
1. Add the GitHub repository as a remote: 
```bash 
git remote add origin https://github.com/<your-username>/comp423-go-tutorial.git
```
Replace ```<your-username>``` with your GitHub username.
2. Check your default branch name with the subcommand ```git branch```. If it's not main, rename it to main with the following command: ```git branch -M main```. Old versions of ```git``` choose the name ```master``` for the primary branch, but these days ```main``` is the standard primary branch name.
3. Push your local commits to the GitHub repository: 
```git push --set-upstream origin main```
4. Back in your web browser, refresh your GitHub repository to see that the same commit you made locally has now been *pushed* to remote. You can use ```git log``` locally to see the commit ID and message which should match the ID of the most recent commit on GitHub. This is the result of pushing your changes to your remote repository.

## Part 2: Setting up the Development Environment
### What is a Development (Dev) Container?

A dev container ensures that your development environment is consistent and works across different machines. At its core, a dev container is a preconfigured environment defined by a set of files, typically leveraging Docker to create isolated, consistent setups for development. Think of it as a "mini computer" inside your computer that includes everything you need to work on a specific project—like the right programming language, tools, libraries, and dependencies.

Why is this valuable? In the technology industry, teams often work on complex projects that require a specific set of tools and dependencies to function correctly. Without a dev container, each developer must manually set up their environment, leading to errors, wasted time, and inconsistencies. With a dev container, everyone works in an identical environment, reducing bugs caused by "it works on my machine" issues. It also simplifies onboarding new team members since they can start coding with just a few steps.

### How are software project dependencies managed?
To effectively manage **software dependencies**, it's important to understand package and dependency management. In most software projects, you rely on external libraries or packages to save time and leverage work that has already been done by others. Managing these dependencies ensures that your project has access to the correct versions of these libraries, avoiding compatibility issues. 

In summary, the ```devcontainer.json``` file specifies configuration for a consistent development environment using a Docker image. 

Now let's establish the development environment for Go. 

### Step 1. Add Development Container Configuration
1. In VS Code, open the ```comp423-go-tutorial directory```. You can do this via: File > Open Folder.
2. Install the **Dev Containers** extension for VS Code. 
3. Create a ```.devcontainer``` directory in the root of your project with the following file inside of this "hidden" configuration directory.
```bash 
.devcontainer/devcontainer.json
```
The ```devcontainer.json``` file defines the configuration for your development environment. Here, we're specifying the following:
    - **name**: A descriptive name for your dev container.
    - **image**: The Docker image to use, in this case, the latest version of a Go environment.
    - **customizations**: Adds useful configurations to VS Code, like installing the Go extension. When you search for VSCode extensions on the marketplace, you will find the string identifier of each extension in its sidebar. Adding extensions here ensures other developers on your project have them installed in their dev containers automatically.
    - **postCreateCommand**: A command to run after the container is created. In our case, it will run ```go mod init github.com/<your-username>/comp423-go-tutorial```. The ```go mod init``` subcommand creates a go.mod file to track your code's dependencies. Currently, the file contains the name of your module and the Go version your dev container supports. But as you add dependencies, the go.mod file will list the versions your code depends on. 
```json
{
    "name": "COMP423 Go Tutorial",
    "image": "mcr.microsoft.com/devcontainers/go:latest",
    "customizations": {
      "vscode": {
        "settings": {},
        "extensions": ["golang.go"]
      }
    },
    "postCreateCommand": "go mod init github.com/<your-username>/comp423-go-tutorial || true"
  }
```
Replace ```<your-username>``` with your GitHub username.
### Step 2. Reopen the Project in a VSCode Dev Container

Reopen the project in the container by pressing ```Ctrl+Shift+P``` (or ```Cmd+Shift+P``` on Mac), typing "Dev Containers: Reopen in Container," and selecting the option. This may take a few minutes while the image is downloaded and the requirements are installed.

Once your dev container setup completes, close the current terminal tab (trash can), open a new terminal pane within VSCode, and try running ```go version``` to see your dev container is running a recent version of Go without much effort! (As of this writing: 1.25 released in January of 2025.)

## Part 3: Creating, running, and compiling a Hello world program
### Step 1. Creating the project 
In your root directory, create a file hello.go in which to write your code. 

### Step 2. Write the program
Paste the following code into your file: 
```go 
package main 

import "fmt"

func main() {
    fmt.Println("Hello 426")
}
```
Let's break the code down: 

- **package main**: declares a main package (a package is a way to group functions, and it’s made up of all the files in the same directory)
- **import “fmt”**: import the fmt package, which contains functions for formatting text, including printing to the console. This package is one of the standard library packages that come with installing Go. 
- **main()**: Implementation of the main function that will print a message to the console. A main function executes by default when you run the main package. 

### Step 3. Run your code
Run the program using the command ```go run .```
```bash 
go run 
```
This compiles and runs the named main Go package. 

### Step 4. Compile your code

While the ```go run``` command is a useful shortcut to compile and run and program when making frequent changes, it doesn’t generate a binary executable. The ```go build``` command compiles the packages, along with their dependencies. 

!!! note 
    ```go build``` doesn’t install the results. The ```go install``` command will compile and install the packages.

1. From the command line in the root directory, run the ```go build``` command to compile the code into an executable. 
```bash 
go build 
``` 
2. Run the new executable directly in the terminal. 
```bash 
./comp423-go-tutorial
```
```go run``` both compiles and runs the program in one step, but doesn’t produce a separate executable file. You would typically use ```go run``` when you’re developing and want to quickly test a Go program without needing to create a standalone executable. ```go build``` will compile and generate an executable file, which you can run separately. This may feel familiar as the build command is similar to the ```gcc``` subcommand for C and C++, combining relocatable object files and libraries into a binary executable object file that can be copied into memory and ran. You would typically use ```go build``` when you want to create a distributable binary that can be executed independently or when preparing your program for deployment. 

### Step 5. Push Changes 
1. Add and commit your changes 
```bash 
git add .
git commit -m "Finished go tutorial"
```
2. Push the changes to GitHub: 
```bash 
git push origin main
```