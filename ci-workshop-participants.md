
1. Pre-requisites: ( Steps to do before the workshop)

    1. https://github.com/HarmeetSokhi/ci-workshop-app/blob/master/docs/pre-requisites.md
    2. Create CircleCI account: https://circleci.com/ (free)
    3. Create heroku account: https://heroku.com/ (free)
    
2. Workshop setup Steps:  

    1. Fork repository:  https://github.com/HarmeetSokhi/ci-workshop-app
    
    2. Clone repository: git clone https://github.com/YOUR_USERNAME/ci-workshop-app

    4. Start Docker on your desktop (Note: Wait for Docker to complete startup before running the subsequent commands. You'll know when startup is completed when the docker icon in your taskbar stops animating)

        1. Build docker image

                # [Mac/Linux users]
                    docker build . -t ci-workshop-app --build-arg user=$(whoami)

                # [Windows users]
                    MSYS_NO_PATHCONV=1 docker build . -t ci-workshop-app --build-arg user=$(whoami)

        2. Start docker container
                # [Mac/Linux users]
                        docker run -it -v $(pwd):/home/ci-workshop-app -p 8080:8080 ci-workshop-app bash

                # [Windows users]
                        winpty docker run -it -v C:\\Users\\path\\to\\your\\ci-workshop-app:/home/ci-workshop-app -p 8080:8080 ci-workshop-app bash
                        # Note: to find the path, you can run `pwd` in git bash, and manually replace forward slashes (/) with double backslashes (\\)

        3. Start a bash shell in a running container when it’s running
                        docker exec -it <container-id> bash
        4. Common commands (run these in the container)
                        # Add some color to your terminal
                            source bin/color_my_terminal.sh

                        # Run unit tests
                            nosetests

                        # Train model
                            python src/train.py

                        # Start flask app
                            python src/app.py

                        # Make requests to your app
                            # 1. In your browser, visit http://localhost:8080
                            # 2. In another terminal in the container, run:
                                    bin/predict.sh http://localhost:8080

                        # You can also use this script to test your deployed application later:
                                bin/predict.sh http://my-app.herokuapp.com
                            
                                        
                                        # You can also use this script to test your deployed application later:
                                bin/predict.sh http://my-app.herokuapp.com

        
    5. Setting up your CD pipeline      

         #.CircleCI: 
                Create circleci project. Visit https://circleci.com/dashboard, login and click on 'Add Projects' on the left panel. Click on 'Set up project' for ci-workshop-app
         #.Heroku
                1.Login to heroku by running: 
                        heroku login 
                        (complete authentication by clicking on the browser. if the browser doesn't open up automatically, you can copy and paste the link manually)
                2.Create a heroku project for app (staging): 
                        heroku create ci-workshop-app-<YOUR_NAME>-staging
                3.Create a heroku project for app (prod): 
                        heroku create ci-workshop-app-<YOUR_NAME>-prod
                        If you encounter problems creating the 2 apps using ther heroku cli, you can create the 2 apps on the heroku website: https://dashboard.heroku.com/new-app


    6. Let's build our CD pipeline!

            #Iteration 1: Hello world
                          Let's create a simple pipeline to run 2 commands: echo 'hello' and echo 'goodbye'
                          
                          #. cp .circleci/config.helloworld.yaml .circleci/config.yaml
                          #. git add .circleci/config.yml
                          #. git commit -m "Creating pipeline to run hello world commands"
                          #. git push -u origin master

            #Iteration 2: Train and test
                        
                        Let's extend to pipeline to 
                            (i) run unit tests, 
                            (ii) train the model, and 
                            (iii) run metrics tests on the model.

                        Your tasks

                            #. In your code editor, open and read the following bash scripts:
                                    #. bin/test.sh
                                    #. bin/train_model.sh
                                    #. bin/test_model_metrics.sh
                            #. Get a feel of what each bash script is doing by running them:
                            #. Start a bash terminal in your container: 
                                docker run -it -v $(pwd):/home/ci-workshop-app -p 8080:8080 ci-workshop-app bash
                            #. In the terminal, run each of the 3 scripts above (e.g. bin/test.sh)

                            #. cp .circleci/config.heroku.v1.yaml .circleci/config.yml
                            #. git add, commit and push your changes to your repository

            #Iteration 3: Deploy to staging and production

                        Your tasks

                            #.In your code editor, open and read the following bash scripts:
                                        bin/deploy_to_heroku.sh - we wrote this script. this is how you can deploy an app to heroku
                                        Procfile - This is a simple shell script which heroku will run when it starts your application

                            #. cp .circleci/config.heroku.reference.yaml .circleci/config.yml
                            #. Replace ci-workshop-app-bob-staging and ci-workshop-app-bob-prod with the names of your staging and prod apps
                            #.Keep the train_and_test configuration which you pasted in your previous task.
                            #.Ensure indentation matches what you pasted in your previous task! Otherwise CircleCI will not be happy.
                            #.git add, commit and push your changes to your repository

            #.Iteration 4: Deploy to production (for real)

                        #.In your terminal, generate a heroku auth token and copy the 'Token' value : 
                                heroku authorizations:create
                        #. On CircleCI webpage, go to your project settings (click on the gear icon on your project) #  Click on 'Environment Variables' on the left panel. Add the following variable:
                                Name: HEROKU_AUTH_TOKEN
                                Value: (paste value created from previous step)
                        #.On CircleCI's workflows page, find the failed workflow and click on 'Rerun'

                         