﻿# DeepWell
Team repo for Wishing Well team - Virtual summer internship 2020


## Running with script (Windows/PowerShell)
First, open powershell and make sure you are allowed to run scripts by entering:

    Set-ExecutionPolicy Unrestricted

You only have to do that one time.
Then you can navigate to the deepwell repository on your computer and build the container by running:

    .\deepwellstart.ps1 -build
Then, to run the container, simply enter:

    .\deepwellstart.ps1 -run <num timesteps>

A handy tip is just to write the first couple of letters "de" and press tab to complete.

You can then see the plot at http://localhost:8080 and the tensorboard result from the training session at http://localhost:7007

### Examples of how you can use the script:
Rebuild docker image (needed to load changes in env):

    .\deepwellstart.ps1 -b

Train the model with 10000 timesteps:

    .\deepwellstart.ps1 -r train 10000
  
Build and retrain the model with 10000 timesteps:

    .\deepwellstart.ps1 -br retrain 10000



    .\deepwellstart.ps1 -br retrain 10000

## Running without script (Non windows)

  
Instructions if you for some reason cannot launch docker using the script. 
(For example if you are on linux/mac)


First, navigate to the deepwell repository on your computer.

Then build the container using:

  

    docker build -t deepwell-app .

  

Then start it with:

  

    docker run -dit --mount type=bind,source="$(pwd)",target=/app -p 0.0.0.0:7007:6006 -p 8080:8080 --name dwrunning deepwell-app


Then start the tensorboard server:

    docker exec -dit dwrunning tensorboard --logdir /usr/src/app/logs/ --host 0.0.0.0 --port 6006

And lastly, run python with

    docker exec -it dwrunning python /app/main.py <text_arg_for_python> <num_arg_for_python>

For information regarding the paramters, see the "running with script" section above.

It should now display the plot at http://localhost:8080/ and the tensorboard result from the training session at http://localhost:7007
  

  

When you have changed your code, you need to remove the old container
  

    docker rm -f dwrunning

  
Then just run again.
  

Note that if you edit the env, you have to build the container again for the changes to take effect. If you are not seeing your changes being applied, try rebuilding.