# Google Compute Engine - [Lab](https://codelabs.developers.google.com/codelabs/cp100-compute-engine/#1)

## IaaS Solution for Deploying VM Instances

### Deployment

* Configure Cloud SDK with default zone
* Create Compute Engien instance
* Create Firewall Rule

### Shell Commands

View a list of available zones in which to create your container cluster

```shell
gcloud compute zones list
```

Configure Cloud SDK with a default zone where <ZONE> is your chosen zone

```shell
gcloud config set compute/zone <ZONE>
```

Review your current Cloud SDK configuration

```shell
gcloud config list
```

Create Compute Engine instance

* --image-family and --image-project: These flags specify the name of the Compute Engine image used to create the instance. In this case, the image is based on the Debian 8 Linux operating system.
* --machine-type: This flag specifies the CPU and memory resources that will be allocated to the instance. 
* --scopes: The scopes flag is used to pass in a list of scopes that should be assigned to a Compute Engine instance. Scopes are used to limit your access to only the methods and user information that your application requires.
* --metadata-from-file: This flag supplies some custom metadata for this particular Compute Engine instance. In this case, the metadata supplies the contents of the startup script you reviewed earlier in the lab.
* --tags: The tags flag allows you to assign arbitrary tags to describe your instance. Tags can be used to associate an instance with other resources, and in this case, is used to establish a relationship with a firewall rule to allow access to the Bookshelf application.

```shell
gcloud compute instances create <PROJECT_NAME> \
  --image-family=debian-8 \
  --image-project=debian-cloud \
  --machine-type=g1-small \
  --scopes userinfo-email,cloud-platform \
  --metadata-from-file startup-script=startup-scripts/startup-script.sh \
  --tags http-server
```

Create Firewall Rule

* default-allow-http-8080: This is the name of your rule. Note that the term ‘default' refers to the name of the network where the rule applies. This is a naming convention that is typically used when creating Compute Engine firewall rules, but you are free to come up with an alternative naming convention.
* --allow: This flag is used to specify the protocol and port for which you are configuring access.
* --source-ranges: This flag specifies what IP addresses can connect to the allowed port(s). In this case a setting 0.0.0.0/0 allows connections from any IP address.
* --target-tags: This flag indicates which tags the rule will apply to. Recall that you applied a tag to the Compute Engine instance you created earlier in this section. The tag specified in the rule must match the tag applied to the instance(s), or you will not be able to access the application.
* --description: Used to supply a human-readable description of what the rule does. This flag is optional.

```shell
gcloud compute firewall-rules create default-allow-http-8080 \
  --allow tcp:8080 \
  --source-ranges 0.0.0.0/0 \
  --target-tags http-server \
  --description "Allow port 8080 access to http-server"
```

Get the output of the serial port for the cp100 instance

```shell
gcloud compute instances get-serial-port-output bookshelf
```

### Third Party Configuration Managament Tools

* Puppet
* Chef
* Salt
* Ansible
* [doc ref](https://cloud.google.com/solutions/google-compute-engine-management-puppet-chef-salt-ansible)

### Other Relevant Topics

* [custome machine types](https://cloud.google.com/compute/docs/instances/creating-instance-with-custom-machine-type)