# Puppet module: ddclient)

## Getting started

This is a Puppet module for ddclient based on the second generation layout ("NextGen") of Example42 Puppet Modules.

Made by Javier Bertoli / Netmanagers

Official site: http://www.netmanagers.com.ar

Official git repository: http://github.com/ajdelgado/puppet-ddclient

Released under the terms of Apache 2 License.

This module uses R.I.Pienaar's concat module as a dependency.

This module requires functions provided by the Example42 Puppi module (you need it even if you don't use and install Puppi)

For detailed info about the logic and usage patterns of Example42 modules check the DOCS directory on Example42 main modules set.

## USAGE - Basic management

* All parameters can be set using Hiera. See the manifests to see what can be set.

* Install ddclient with default distro's settings

        class { 'ddclient': }

* You can install and configure a single dDNS host/provider using a template (ie., the included one):

        class { 'ddclient': 
          hosts_config => 'file',
          template    => 'ddclient/ddclient.conf.erb',
          server      => 'ddns_provider',
          login       => 'myuser',
          password    => 'secret',
          protocol    => 'ddns_prot',
          hostname    => 'my.host.name',
        }

* Or you can configure multiple hosts using host\_config => 'concat' and ddclient::host define:

        class { 'ddclient': 
          hosts_config   => 'concat',
        }

        ddclient::host { 'my_ddns.hostname.com':
          server    => 'one.ddns.provider.com',
          login     => 'my_account',
          password  => 'secret',
          protocol  => 'dyndns2',
        }

* Install a specific version of ddclient package

        class { 'ddclient':
          version => '1.0.1',
        }

* Disable ddclient service.

        class { 'ddclient':
          disable => true
        }

* Remove ddclient package

        class { 'ddclient':
          absent => true
        }

* Enable auditing without without making changes on existing ddclient configuration *files*

        class { 'ddclient':
          audit_only => true
        }

* Module dry-run: Do not make any change on *all* the resources provided by the module

        class { 'ddclient':
          noops => true
        }


## USAGE - Overrides and Customizations
* Use custom sources for main config file 

        class { 'ddclient':
          source => [ "puppet:///modules/example42/ddclient/ddclient.conf-${hostname}" , "puppet:///modules/example42/ddclient/ddclient.conf" ], 
        }


* Use custom source directory for the whole configuration dir

        class { 'ddclient':
          source_dir       => 'puppet:///modules/example42/ddclient/conf/',
          source_dir_purge => false, # Set to true to purge any existing file not present in $source_dir
        }

* Use custom template for main config file. Note that template and source arguments are alternative. 

        class { 'ddclient':
          template => 'example42/ddclient/ddclient.conf.erb',
        }

* Automatically include a custom subclass

        class { 'ddclient':
          my_class => 'example42::my_ddclient',
        }


## USAGE - Example42 extensions management 
* Activate puppi (recommended, but disabled by default)

        class { 'ddclient':
          puppi    => true,
        }

* Activate puppi and use a custom puppi_helper template (to be provided separately with a puppi::helper define ) to customize the output of puppi commands 

        class { 'ddclient':
          puppi        => true,
          puppi_helper => 'myhelper', 
        }

* Activate automatic monitoring (recommended, but disabled by default). This option requires the usage of Example42 monitor and relevant monitor tools modules

        class { 'ddclient':
          monitor      => true,
          monitor_tool => [ 'nagios' , 'monit' , 'munin' ],
        }

## CONTINUOUS TESTING

Travis {<img src="https://travis-ci.org/netmanagers/puppet-ddclient.png?branch=master" alt="Build Status" />}[https://travis-ci.org/netmanagers/puppet-ddclient]
