# Role Ansible pour la gestion de Tomcat

```yaml
---
- name: Converge
  hosts: all
  become: true
  gather_facts: true

  vars:
    # tomcat_address: "127.0.0.1"
    tomcat_instances:
      - name: "tomcat"
      # - name: "tomcat-version-7"
      #   version: 7
      #   shutdown_port: 8007
      #   non_ssl_connector_port: 8082
      #   ssl_connector_port: 8445
      #   ajp_port: 8011
      # - name: "tomcat-version-8"
      #   version: 8
      #   shutdown_port: 8008
      #   non_ssl_connector_port: 8083
      #   ssl_connector_port: 8446
      #   ajp_port: 8012
      # - name: "tomcat-version-9"
      #   version: 9
      #   shutdown_port: 8019
      #   non_ssl_connector_port: 8084
      #   ssl_connector_port: 8447
      #   ajp_port: 8013
      # - name: "tomcat-specific"
      #   user: "specificuser"
      #   group: "specificgroup"
      #   shutdown_port: 8020
      #   shutdown_pass: shutme
      #   non_ssl_connector_port: 8085
      #   ssl_connector_port: 8448
      #   ajp_port: 8014
      #   xms: 256M
      #   xmx: 512M
      # - name: "tomcat-with-wars"
      #   shutdown_port: 8021
      #   non_ssl_connector_port: 8086
      #   ssl_connector_port: 8449
      #   ajp_port: 8015
      #   wars:
      #     - url: https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample/sample.war
      #     - url: "https://github.com/aeimer/java-example-helloworld-war/raw/master/dist/helloworld.war"
      #       context_name: "my-helloworld"  # Store helloword.war as my-helloworld
      # - name: "tomcat-java_opts"
      #   shutdown_port: 8022
      #   non_ssl_connector_port: 8087
      #   ssl_connector_port: 8449
      #   ajp_port: 8016
      #   java_opts:
      #     - name: UMASK
      #       value: "0007"
      # - name: "tomcat-with_lib"
      #   shutdown_port: 8023
      #   non_ssl_connector_port: 8088
      #   ssl_connector_port: 8450
      #   ajp_port: 8017
      #   libs:
      #     - url: "https://search.maven.org/remotecontent?filepath=io/prometheus/simpleclient/0.6.0/simpleclient-0.6.0.jar"
      # - name: "tomcat-access-logs"
      #   shutdown_port: 8024
      #   non_ssl_connector_port: 8089
      #   ssl_connector_port: 8451
      #   ajp_port: 8018
      #   access_log_enabled: true
      #   access_log_directory: "my-logs"
      #   access_log_prefix: my-access-logs
      #   access_log_suffix: ".log"
      #   access_log_pattern: "%h %l %u %t &quot;%r&quot; %s %b"
      # - name: "tomcat-config-files"
      #   shutdown_port: 8025
      #   non_ssl_connector_port: 8090
      #   ssl_connector_port: 8452
      #   ajp_port: 8019
      #   ajp_secret: "SoMe-SeCrEt"
      #   config_files:
      #     - src: "{{ role_path }}/files/dummy.properties"
      #       dest: "./"
      #       mode: "0644"
      # - name: "tomcat-context"
      #   shutdown_port: 8026
      #   shutdown_pass: shutme
      #   non_ssl_connector_port: 8091
      #   ssl_connector_port: 8453
      #   ajp_port: 8020
      #   context: |
      #     <Context>
      #       <Manager className="org.apache.catalina.session.PersistentManager" maxIdleSwap="10000" maxIdleBackup="10000" />
      #     </Context>
      # - name: "tomcat-empty"
      #   shutdown_port: 8027
      #   shutdown_pass: shutme
      #   non_ssl_connector_port: 8092
      #   ssl_connector_port: 8454
      #   ajp_port: 8021
      #   cleanup_enabled: true
      #   remove_webapps:
      #     - docs
      #     - examples
      #     - host-manager
      #     - manager
      #     - ROOT
      # - name: "tomcat-java_home"
      #   java_home: "/opt/java/jdk-17"

  roles:
    - role: robertdebock.tomcat
```


## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/robertdebock/ansible-role-tomcat/blob/master/defaults/main.yml):

```yaml
---
# defaults file for tomcat

# Some "sane" defaults.
tomcat_name: tomcat
tomcat_directory: /opt
tomcat_version: 10
tomcat_user: tomcat
tomcat_group: tomcat
tomcat_xms: 512M
tomcat_xmx: 1024M
tomcat_non_ssl_connector_port: 8080
tomcat_ssl_connector_port: 8443
tomcat_shutdown_port: 8005
tomcat_shutdown_pass: SHUTDOWN
tomcat_ajp_enabled: true
tomcat_ajp_port: 8009
tomcat_ajp_secret: "SoMe-SeCrEt"
tomcat_jre_home: /usr
tomcat_service_state: started
tomcat_service_enabled: true
# You can bind Tomcat to a specified address globally using this variable, or
# in the `tomcat_instances`. The `tomcat_instances.address` is more specific
# so it takes priority over `tomcat_address`.
tomcat_address: "0.0.0.0"

# Configure tomcat access logs
tomcat_access_log_enabled: true
tomcat_access_log_directory: logs
tomcat_access_log_prefix: localhost_access_log
tomcat_access_log_suffix: ".txt"
tomcat_access_log_pattern: "%h %l %u %t &quot;%r&quot; %s %b"

# This role allows multiple installations of Apache Tomcat, each in their own
# location, potentially of different version.
# This is done by defining a "tomcat_instances" where "name:" is a unique
# identifier of an instance.
# The default tomcat_instances is one instance using the defaults described
# in defaults/main.yml.
tomcat_instances:
  - name: "{{ tomcat_name }}"
    version: "{{ tomcat_version }}"
    user: "{{ tomcat_user }}"
    group: "{{ tomcat_group }}"
    xms: "{{ tomcat_xms }}"
    xmx: "{{ tomcat_xmx }}"
    non_ssl_connector_port: "{{ tomcat_non_ssl_connector_port }}"
    ssl_connector_port: "{{ tomcat_ssl_connector_port }}"
    shutdown_port: "{{ tomcat_shutdown_port }}"
    ajp_enabled: "{{ tomcat_ajp_enabled }}"
    ajp_port: "{{ tomcat_ajp_port }}"
    ajp_secret: "{{ tomcat_ajp_secret }}"
    # You can pick an address per instance:
    # address: "127.0.0.1"
    packet_size: 8192
    java_opts:
      - name: JRE_HOME
        value: "{{ tomcat_jre_home }}"
    access_log_enabled: "{{ tomcat_access_log_enabled }}"
    access_log_directory: "{{ tomcat_access_log_directory }}"
    access_log_prefix: "{{ tomcat_access_log_prefix }}"
    access_log_suffix: "{{ tomcat_access_log_suffix }}"
    access_log_pattern: "{{ tomcat_access_log_pattern }}"
    service_state: "{{ tomcat_service_state }}"
    service_enabled: "{{ tomcat_service_enabled }}"

# The explicit version to use when referring to the short name.
tomcat_version7: "7.0.109"
tomcat_version8: "8.5.100"
tomcat_version9: "9.0.100"
tomcat_version10: "10.1.36"

# The location where to download Apache Tomcat from.
tomcat_mirror: "https://archive.apache.org"
```

