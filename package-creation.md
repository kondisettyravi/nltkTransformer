
### Set up GitHub account

#### Create new GitHub account
1. Go to https://github.com/join in a web browser. 
2. Enter your personal details.
3. Choose the plan of your choice
4. Verify your email

#### Create a repository in GitHub

1. In the upper-right corner of any page, click , and then click New repository.
2. Drop-down with option to create a new repository
3. In the Owner drop-down, select the account you wish to create the repository on.
4. Type a name for your repository, and an optional description.
5. Choose to make the repository either public or private. Public repositories are visible to the public, while private repositories are only accessible to you, and people you share them with. For more information, see "Setting repository visibility."
6. There are a number of optional items you can pre-populate your repository with. 
    * You can create a README, which is a document describing your project. For more information, see "About READMEs."
    * You can create a .gitignore file, which is a set of ignore rules. For more information, see "Ignoring files."
7. When you're finished, click Create repository.

### Setup GitHub locally
1. Open the terminal.
2. Enter ls -al ~/.ssh to see if existing SSH keys are present:

    `$ ls -al ~/.ssh`

    Above command lists the files in your .ssh directory, if they exist
3. Check the directory listing to see if you already have a public SSH key.
By default, the filenames of the public keys are one of the following:

* id_dsa.pub
* id_ecdsa.pub
* id_ed25519.pub
* id_rsa.pub
* If you don't have an existing public and private key pair, or don't wish to use any that are available to connect to GitHub, then generate a new SSH key.
* If you see an existing public and private key pair listed (for example id_rsa.pub and id_rsa) that you would like to use to connect to GitHub, you can add your SSH key to the GitHub account.


#### Generate new SSH key
1. Open the terminal.
2. Paste the text below, substituting in your GitHub Enterprise email address.
    `$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`
    
    This creates a new ssh key, using the provided email as a label.

3. When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location.
    `Enter a file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]`
    
    `Enter a file in which to save the key (/home/you/.ssh/id_rsa): [Press enter]`
    
4. At the prompt, type a secure passphrase. I do not personally use any keys
    `Enter passphrase (empty for no passphrase): [Type a passphrase]`
    
    `Enter same passphrase again: [Type passphrase again]`
    
#### Add SSH Key to your GitHub account
1. Copy the SSH key to your clipboard.
    If your SSH key file has a different name than the example code, modify the filename to match your current setup. When copying your key, don't add any newlines or whitespace.
    
    `$ pbcopy < ~/.ssh/id_rsa.pub`
    
    Copies the contents of the id_rsa.pub file to your clipboard
    
    `#Tip#: If pbcopy isn't working, you can locate the hidden .ssh folder, open the file in your favorite text editor, and copy it to your clipboard.`

2. In the upper-right corner of any page, click your profile photo, then click Settings icon in the user bar
3. In the user settings sidebar, click SSH and GPG keys.
4. Click New SSH key or Add SSH key.
5. In the "Title" field, add a descriptive label for the new key. 
6. Paste your key into the "Key" field.
7. Click Add SSH key.
8. If prompted, confirm your GitHub Enterprise password.


#### Clone a repository

1. On GitHub, navigate to the main page of the repository.
2. Under the repository name, click Clone or download.
3. To clone the repository using an SSH key, including a certificate issued by your organization's SSH certificate authority, click Use SSH, then copy the URL
4. Open Terminal.
5. Change the current working directory to the location where you want the cloned directory.
6. Type git clone, and then paste the URL you copied earlier.
    `$ git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY`
    Press Enter to create your local clone.
    `$ git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY`


### Setup PyPI

1. Open `pypi.org` in the browser.
2. On the top right, click register
3. Enter details
4. Verify email

#### Generate API token
5. Login to PyPI
6. On the top right, click user name then click on Account Settings
7. Scroll down to `API tokens`
8. Click on `Add API token`
9. Give token name and select the scope to entire project.
##### Copy the token and save it at a safe place. In case you miss it, just repeat the steps in this section. But, do not forget to change the keys wherever required.

### Setup code for packaging

1. After cloning a repository, change directory to that folder. We would be calling this folder `base directory` going forward
2. Create folder that denotes the work you want to place in a module
3. Create one class per file and name the file with the name of the class
4. `cd` to base directory and create a file `setup.py`
5. Paste the below code in the file and make changes as required
     ```
   import setuptools
     
     with open("README.md", "r") as fh:
         long_description = fh.read()
     
     setuptools.setup(
         name="example-pkg-YOUR-USERNAME-HERE", # Replace with your own username
         version="0.0.1",
         author="Example Author",
         author_email="author@example.com",
         description="A small example package",
         long_description=long_description,
         long_description_content_type="text/markdown",
         url="https://github.com/pypa/sampleproject",
         packages=setuptools.find_packages(),
         classifiers=[
             "Programming Language :: Python :: 3",
             "License :: OSI Approved :: MIT License",
             "Operating System :: OS Independent",
         ],
         python_requires='>=3.6',
     )
    ```

6. Make sure you have the latest versions of setuptools and wheel installed
   `python3 -m pip install --user --upgrade setuptools wheel`
7. Now run this command from the base directory
   `python3 setup.py sdist bdist_wheel`
8. In order to upload the code to PyPI index, you need `twine`. 
    * In order to install twine, run `pip install twine`
    * In order to update twine, run `python3 -m pip install --user --upgrade twine`
9. Once done, run twine to upload all archives under dist
    `python3 -m twine upload dist/*`
10. Add all the code files to git by using the command
    `git add <file names followed by a space>`
11. Then commit the files by using 
    `git commit -m "appropriate message with changes"`
12. Push the code to GitHub
    `git push`

#Now all your files would be available in GitHub and you can also see the index listed in your PyPI account.