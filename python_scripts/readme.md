## Setting Up Your Python Environment
If you don't already have Python installed on your system, then download it from [here](https://www.python.org/downloads/). Then follow these steps to clone the GitHub repository for this course and set up a Python Virtual Environment to run the scripts in (they assume you also have [Git](https://github.com/git-guides/install-git) installed):

1. Clone the NetBox Zero to Hero GitHub repository and change into the `python_scripts` directory:
```
git clone https://github.com/netbox-community/netbox-zero-to-hero.git
cd netbox-zero-to-hero/python_scripts
```

2. Create a new Python Virtual Environment and activate it:
```
python3 -m venv .
source bin/activate
```
3. Install the `python-dotenv` module which will be used to load environment variables from a `.env` file. This allows you to use credentials and API tokens in your scripts, without having them appear in the actual code:
```
pip3 install python-dotenv
```
5. Install the `requests` module which is used to make the API requests
```
pip3 install requests
```
6. Rename file `.env.example` to `.env` **Important** - remember to add`.env` to your `.gitignore` file to avoid uploading the file with your api token in it to your GitHub repository, if you are pushing code back to your own public GitHub repository.
```
mv .env.example .env
```
7. Edit the file `.env` and add your API token
```
api_token = "<YOUR NETBOX API TOKEN>"
```
8. When you have finished working with the scripts, deactivate the Python Virtual Environment:
```
deactivate
```