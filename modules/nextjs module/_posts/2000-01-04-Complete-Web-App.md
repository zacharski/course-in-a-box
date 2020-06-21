## Complete Web Application

<iframe width="560" height="315" src="https://www.youtube.com/embed/2XRc-_5hrJY" frameborder="0" allowfullscreen></iframe>

There are several good methods for combining front and backend code to create a complete web application. I am outlining just one of them.


## General Idea

Here is the general idea. We will have the backend code running on port 8080 and the front end next code running on port 3000. We will use NGINX as a reverse proxy server. To have NGINX route the request to the correct server we will use a common prefix for all requests going to the backend server. So, for example, we can use `/api`. So if NGINX gets the request `http://oursite.com/api/info?name=Saddle` it knows to route it to the backend server. If its gets a request that starts without the `/api` it will route it to the front end server. 


## Backend server
We need to modify the backend server code to add that prefix. For example,


    app.get("/api/info", async (req, res) => {
            console.log(req.query.q);
            try {
                    const template = "SELECT * from campgrounds WHERE name = $1";
                    const response = await pool.query(template, [
                            req.query.q
                    ]);
                    //console.log(response);
                    if (response.rowCount == 0) {
                            res.sendStatus(404);
                    }
                    res.json(response.rows[0]);
            } catch (err) {
                    console.error("Error running query " + err);
            }
    });

So here we are adding the `/api` prefix.

That's it. We can start it with

	pm2 start server.js


(make sure you are in the backend server directory)

## Our Next.js front end app    

We don't need to make any changes to our Next.js app. First we need to build the app

	npm run build

And then have pm2 start it:

	pm2 start npm --name "next" -- start

##  Modify the NGINX default file

Next we need to modify the file `/etc/nginx/sites-available/default` 


    server {
            listen 80 default_server;
            listen [::]:80 default_server;
            root /var/www/html;
            # Add index.php to the list if you are using PHP
            index index.html index.htm index.nginx-debian.html;
            server_name _;
                location /api {
            proxy_pass http://localhost:8080;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
            location / {
            proxy_pass http://localhost:3000;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }


Once you save the file you can check for syntax errors:


	sudo nginx -t

and if there are none, restart the server


	sudo systemctl restart nginx

That should do it!