# Development

## Contributing

Thank you for considering contributing to An Otter Wiki! If you ran into a problem or want to request a feature, please check the existing [issues](https://github.com/redimp/otterwiki/issues) in case someone else had a similar request and support their case or open a new issue. If you want to contribute code, design or documentation directly, please [fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo) the [repository](https://github.com/redimp/otterwiki) on <i class="fab fa-github"></i> github. Please make sure all tests pass and submit your changes as a pull request.

## Setting up a development environment

1. Install the prerequisites
	- Debian / Ubuntu 
	```bash
    apt install git build-essential python3-dev python3-venv
    ```
    - RHEL8 / Fedora / Centos8 / Rocky Linux 8
    ```bash
    yum install make python3-devel
    ```
    - MacOS 
    ```bash
    brew install python3
    ```
2. Clone the repository (in case you've created a fork, replace the url with your repository) and enter the directory
	```bash
    git clone https://github.com/redimp/otterwiki.git && cd otterwiki
    ```
3. Create and initialize the repository where the data is stored
	```bash
	mkdir -p app-data/repository
	# initialize the empty repository
	git init app-data/repository
	```
4. Create a minimal configuration file
	```bash
    echo "REPOSITORY='${PWD}/app-data/repository'" >> settings.cfg
	echo "SQLALCHEMY_DATABASE_URI='sqlite:///${PWD}/app-data/db.sqlite'" >> settings.cfg
	echo "SECRET_KEY='$(echo $RANDOM | md5sum | head -c 16)'" >> settings.cfg
    ```
5. Run `make` to setup a virtual environemnt and run a local server on port 8080
	```bash
    make debug
    ```
6. Open the wiki in your browser <http://127.0.0.1:8080>. You can create and edit pages as anonymous user without any further configuration. Please note: This setup is not for production use!
7. While you edit the code, flask will automatically reload the server.
8. Run tests
	```bash
	make test
	```
9. Run coverage tests
	```bash
	make coverage
	```
	and check `coverage_html/index.html` for detailed information.
