**Server Setup - Step by Step**

**Prerequisites:**

1. Implemented application in Python's Flask framework, Jinja2 templates, SQLite3 database, Leaflet.js map, Tailwind CSS.

2. Created an AWS Account and created an EC2 instance having CentOS Operating System.

3. Implemented an Elastic IP Instance.

4. Associated public IP Address of EC2 instance with the Elastic IP address.

5. Initialized Route 53 and created a zone.

6. Bought a domain name from TrueHost providers in Kenya.

**Actual Work:**

- Host your IS Project 1 in Digital Ocean.

    - Used SSH to connect to the remote CentOS EC2 instance:

        ```
        ssh -i <our_key> centos@<our_public_ip_address>
        ```

    - Updated the packages on the EC2 instance:

        ```
        sudo yum update
        ```

    - Installed the following packages:

        ```
        sudo yum install git python3 nginx
        ```

    - Created a Virtual Environment for the Python application:

        ```
        python3 -m venv venv
        source venv/bin/activate
        ```

    - Used git clone to get a local copy of the project from GitHub
    - Installed relevant packages required by the application:

        ```
        pip install -r requirements.txt
        ```

    - Installed gunicorn package that will serve requests and responses for our application:

        ```
        pip install gunicorn
        ```

    - Ran gunicorn server in the background with the server bound at 0.0.0.0:8000 and with 4 workers:

        ```
        gunicorn -w 4 -b 0.0.0.0:8000 flasky:app --daemon
        ```

    - Tested whether it was working using:

        ```
        curl 127.0.0.1:8000
        ```

    - Activated the nginx server:

        ```
        sudo systemctl start nginx
        ```

    - Checked the status of the nginx server:

        ```
        sudo systemctl status nginx
        ```

    - Configured our nginx server to listen at port 80 and be a reverse proxy for our gunicorn server. Create a file in /etc/nginx/conf.d/myapp.conf with the necessary configuration.

    - Checked our configurations for syntax:

        ```
        sudo nginx -t
        ```

    - Restarted our nginx server:

        ```
        sudo systemctl restart nginx
        ```

    - Checked the status of nginx:

        ```
        sudo systemctl status nginx
        ```

    - Tested whether it was working on the browser.

    - Made some changes since CentOS does not directly allow nginx to communicate with the gunicorn server. Ensure that SELinux policies or firewalls are correctly configured.

    - Checked whether everything was okay, and we could see it in the browser.

- Configure DNS data (Domain name must be valid).

    - Created a hosted zone in AWS Route 53 called climatecns.co.ke.

    - Created NS and SOA records in the hosted zone.

    - We were assigned DNS servers that will be used in mapping the IP address of the Elastic IP address to our domains and subdomains.

    - The Elastic IP Address was associated with our CentOS EC2 instance so that if the public IP Address of our instance changes, we are not affected as end users.

    - Created 2 A records (primary.climatecns.co.ke and climatecns.co.ke) that were mapped to the Elastic IP Address.

    - Copied our DNS servers to TrueHost, our domain name provider.

    - It took time for propagation to take place.

- Configure secure SSL certificate (use the three months trial OpenSSL certificate). Your web application should be secured with HTTPS protocol.

    - **In progress**

- Sensitive data in the database i.e. passwords must be hashed securely. Inputs validation in the HTML forms.

    - In our Python code, we used pbkdf2 encryption: we had two functions, generate_password_hash(password) that returned a hashed password for storage and check_password_hash(password_hash, password) that checked whether the given password was valid.

- Creative or | and innovative idea.

    - Our idea was creative. It is one that aims to combat climate change by advocating for afforestation. It also brings a community of enthusiasts on one platform where progress can be seen. Users register, log in, and can do the following:

        - Add records of trees they planted.

        - Creating events that they organize for all to see.

        - View their own records of trees planted.

        - View their created events.

        - View all records.

        - View all events.

        - View all records on a map (used JavaScript, Leaflet library for maps, and HTML).

        - View all events on a map (used technologies mentioned).
