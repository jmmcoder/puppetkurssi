  class apache2 {

          # Ensure the latest apache2 package is installed
          package { "apache2":
                  ensure  => latest
          }

          # Ensure apache2 is running and start on boot
          service { "apache2":
                  ensure  => "running",
                  enable  => "true",
                  require => Package["apache2"]
          }

          # Remove the index.html in /var/ww/html directory
          file { "/var/www/html/index.html":
                  ensure  => "absent"
          }

          # Ensure public_html directory exists in skel directory
          file { "/etc/skel/public_html":
                  ensure  => "directory"
          }

          # Enable apache2 userdir
          file { "/etc/apache2/mods-enabled/userdir.load":
                  ensure  => "link",
                  target  => "/etc/apache2/mods-available/userdir.load",
                  notify  => Service["apache2"],
                  require => Package["apache2"]
          }

          file { "/etc/apache2/mods-enabled/userdir.conf":
                  ensure  => "link",
                  target  => "/etc/apache2/mods-available/userdir.conf",
                  notify  => Service["apache2"],
                  require => Package["apache2"]
          }

  }
