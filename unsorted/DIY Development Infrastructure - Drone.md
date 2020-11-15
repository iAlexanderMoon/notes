drone-ui

drone server needs a name:


https://github.com/drone
https://medium.com/faun/setup-a-drone-cicd-environment-on-docker-with-letsencrypt-69b259d398fb
https://webhookrelay.com/blog/2019/02/11/using-drone-for-simple-selfhosted-ci-cd/

Digital-Ocean


doctl create 

and 2375/2376 for unencrypted/encrypted communication with the Docker daemon, respectively.

DRONE_SERVER_HOST=ci.example.com
DRONE_SERVER_PROTO=https


DRONE_GITHUB_CLIENT_ID=XXXXXXXXX
DRONE_GITHUB_CLIENT_SECRET=XXXXXXXXXXXX


DRONE USER CREATION

DRONE_USER_CREATE=username:xxxxx,machine:false,admin:true
DRONE_USER_FILTER=xxxxx


    Server is responsible for authentication, repository configuration, users, secrets and accepting webhooks.
    Agent are receiving build jobs and actually run your workflows.
	
No need to configure NAT, Drone server barely uses any resources so it can run on cheap hardware.
Agents are decoupled and can be running on your home/office machines. By connecting to them over the tunnels, agent configuration remains the same, no matter where they are deployed (external cloud provider or internal office).


So drone server needs to be addressable.


	
	For Drone user to be added via github and to limit them
	
	For Drone user to generate docker images in the pipeline to be used later in the pipeline:
	Drone server must set the git repository as trusted. Can only be done by admin.
	
	 
	For Drone to send data back to github release:
	
	generate personnal access token in github
	add as a secret to the drone server. example:

	  