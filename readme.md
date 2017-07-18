# Handy Nagios Plugins
Quick and simple bash scripts plugins that conform to Nagios' plugin API.

## check_memory

Uses Ubuntu's native `landscape-sysinfo` command to get the current memory usage percentage. Accepts a warning threshold percentage and a critical threshold percentage. Best used with **NRPE**.

## check_url

Queries a given URL using the `curl` command to measure the total response time to fetch it. Accepts a URL argument to query, a warning threshold in seconds and a critical threshold in seconds.

## check_nav

Queries the SQL Server database for **Microsoft Business Dynamics Nav 2009 R2** to get the number of active users/sessions. Requires `tsql` to be installed.

## Installation

On Ubuntu/Linux, copy all the plugins to `/usr/lib/nagios/plugins/` and use `chmod 755` to make them executable if needed. 

On OS X, depending on how you installed Nagios, you can copy them to the same path as Linux or if you're using **MacPorts**, copy plugins to `/opt/local/libexec/nagios/`. Use `chmod 755` to make them executable if needed.

In your Nagios `commands.cfg` file or any other `.cfg` you use for your commands, add the following commands for the plugins:

	define command{
    	command_name    check_memory
    	command_line    $USER1$/check_memory -w $ARG1$ -c $ARG2$
    }

    define command{
    	command_name    check_url
    	command_line    $USER1$/check_url -u $ARG1$ -w $ARG2$ -c $ARG3$
    }
    
    define command{
    	command_name    check_nav
    	command_line    $USER1$/check_nav -w $ARG1$ -c $ARG2$
    }

And use the commands in services as follows:

	define service{
		use                 generic-service
		host_name           ExampleHost
		service_description Check Memory
		check_command       check_memory!50!70
	}

	define service{
		use                 generic-service
		host_name           ExampleHost
		service_description Check URL
		check_command       check_url!http://example/!3!6
	}
	
	define service{
		use                 generic-service
		host_name           ExampleHost
		service_description Check NAV Users
		check_command       check_nav!20!30
	}
