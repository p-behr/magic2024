# MAGiC 2024 - PHP Workshop



## ➡️ Setup (comms check)
1. In your /home/{*your_userid*} directory, create a directory called `magic2024`
2. Inside /home/{*your_userid*}/magic2024, create a directory called `htdocs`
3. Inside /home/{*your_userid*}/magic2024/htdocs, create a file called `.htaccess`  
    ❗ there's a period at the front of the filename
4. Inside /home/{*your_userid*}/magic2024/htdocs, create a file called `index.php`

    Your file structure should look like this:   
    ![file structure](/images/file_structure_1.PNG)

5. Paste the following into `.htaccess`
    ```
    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^ index.php [QSA,L]
    ```
    This Apache rewrite rule causes all incoming HTTP requests to be sent to `index.php`.  
    This single point of entry into the application is often called a "front-controller".  
    All requests will be handled by the front-controller first, which can then dispatch the request to other parts of the application as needed.

6. Paste the following into `index.php`
    ```
    <?php
    echo('{your_userid} Web App');
    ```
    ⚠️ Don't actually type "{*your_userid*}, use your IBM i user profile name.  
    Line 1: All PHP files start with `<?php`  
    Line 2: The `echo` function will write the text you provide to the webpage.  
    - PHP doesn't actually write directly to the webpage; PHP writes to the HTTP response, which is sent back to the browser, and the browser displays the text.

7. Open your browser and go to `http://magic.magic-ug.org:{your_port}`

    ⚠️ Don't actually type "{*your_port*}";  
    ❗❗ You should have received a specific port number for the workshop (i.e. 10517) and should use that number. ❗❗  
    If you don't use the correct port number your requests won't work, or you won't be using *your* application.  
    <br>
    When you press &lt;Enter&gt; in the browser, it will make a GET request to your port number (which is where *your* application lives).  
    That HTTP request will be sent to `index.php`, which should output "{*your_userid*} Web App".

    Ignore any errors about the site not being secure (we know it's not using TLS).


    Your webpage should look like this:  
    ![file structure](/images/web_app_1.PNG)


### 🚀 Congratulations!
If you see a webpage like that, it's success!  
You have made an HTTP request to your application, processed the request with PHP, and sent back an HTTP response!

You are ready to move on to the next section!
