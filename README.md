# 12 factor app methodology

    • Codebase
        ◦ Have one codebase
            ▪ Multiple services should not have same codebase
    • Dependencies
        ◦ Explicitly declare and isolate dependencies
            ▪ Eg, in case of python, these dependencies are typically listed in a file “requirements.txt”
            ▪ Docker containers help create reproducible virtual environments which very finely help isolate dependencies 
    • Config
        ◦ Store config in the environment
        ◦ Use env file, it allows us to use different cofigurations for different deployments such as dev, staging, prod, etc
    • Backing Services
        ◦ Treat backing services as attached resources
            ▪ Eg 
                • S3 to store images
                • smtp mail service
                • Redis as a caching service
            ▪ Application should not have anything in our code that is specific to the backing service, if we point our service to a different instance, it should work perfectly...so, no matter if the redis service is local or cloud managed servcie or redis managed service, our application should work properly 
    • Build, release, run
        ◦ to strictly separate build, run and release stages
        ◦ Stages
            ▪ Build
                • converting from code to binary or executable is done through build process
                • eg docker build command for building mage of our application
            ▪ Release
                • Once built, the docker image file along with config becomes the release object
                • Every release should have a release id
            ▪ Run
    • Processes:
        ◦ Sticky sessions are a violation
        ◦ 12 factor processes are stateless and share nothing
        ◦ to execute app as one or more stateless processes
            ▪ For horizontal scaling of servers to work efficiently, we must build our app as an independent stateless app
        ◦ Walk through:
            ▪ Session info is stored on the server to verify whether the user is logged out
            ▪ if this info is stored in process memory, then the user might be considered logged out as this info might not be there in another process
            ▪ Now, there are load balancers that are session aware and redirect the user to same process each time and this is called sticky sessions. But this would not be worthwhile if the process crashes and all stored data is lost
            ▪ That’s why we should not store anything in process and store it in some backing service
            ▪ This way all data and info is stored in a placed that is easily accessible by all processes
    • Port Binding
        ◦ Export services via port binding
        ◦ Our app exports HTTP as a service by binding to a specific port
        ◦ Doesn’t rely on a specific web server to function
    • Concurrency
        ◦ to sclae out via the process model
    • Disposability
        ◦ to maximize robustness with fast startup and graceful shutdown
        ◦ Processes are disposable ie they can be started and stopped at moment’s notice
        ◦ Processes should shutdown racefully when receive a SIGTERM signal from process manager
        ◦ Docker sends Sigterm signal, if container doesn’t stop, Docker sends SIGKILL signal after a grace period
    • Dev/prod parity
        ◦ to keep development staging and production as similar as possible
        ◦ 12 factor app is designed for continuous deployment by keeping the difference between developement and production small
        ◦ 12 factor app resists the urge to have different backing services on development and prod
    • Logs
        ◦ Treat logs as events streams 
        ◦ A 12 factor app never concerns itself with routing or storage of its output stream
        ◦ Store logs in a centralized location in a structured format
        ◦ 12 factor app must not try to write to a specific file or be tied to a specific logging solution. 12 factor apps must write to a standard out or a local file in a structured Json format that can be used by an agent to transfer to a centralized location for consolodation and analysis purpose
        ◦ eg ELK and splunk
    • Admin Processes
        ◦ to run admin or mgmt tasks as one-off processes
        ◦ 12 factor app should have an admin process such as database migrations or server restart be run as separate process or application. However, it should run on identical systems as app running in production enviroment and should be automated, scalable and reproducible|

Credits: Notes made from Kodekloud's course on 12 factor methodology
