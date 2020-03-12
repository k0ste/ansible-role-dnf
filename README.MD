# ansible-dnf

Deploy dnf configuration and dnf repos.

## Requirements

* Ansible 2.8+;

## Example configuration

```yaml
---
# DNF is the next upcoming major version of YUM, a package manager for
# RPM-based Linux distributions.

# Delete existing dnf repos. This option will purge ALL present yum repos in
# 'dnf_repos_dest'.
dnf_drop_exists_repos: 'false'

dnf:
# Add or remove DNF repositories in RPM-based Linux distributions.
# All options pass to 'yum_repository' module. For all available options please
# consult with doc of 'yum_repository' ansible module.
- repos:
  - name: 'ceph_stable'
    description: 'Ceph Stable repo'
    baseurl: 'https://download.ceph.com/rpm-octopus/el8/$basearch'
    gpgcheck: 'yes'
    gpgkey: 'https://download.ceph.com/keys/release.asc'
# The 'main' section must exist for DNF to do anything. It consists of the
# following options:
  main:
# The architecture used for installing packages. By default this is
# auto-detected. Often used together with 'ignorearch' option.
  - arch: ''
# If enabled dnf will assume 'yes' where it would normally prompt for
# confirmation from user input (see also 'defaultyes'). Default is false.
    assumeyes: 'false'
# Automatic check whether there is installed newer kernel module with security
# update than currently running kernel. Default is true.
    autocheck_running_kernel: 'true'
# When upgrading a package, always try to install its highest version available,
# even only to find out some of its deps are not satisfiable. Enable this if
# you want to experience broken dependencies in the repositories firsthand. The
# default is false.
    best: 'false'
# Path to a directory used by various DNF subsystems for storing cache data.
# Has a reasonable root-writable default depending on the distribution. DNF
# needs to be able to create files and directories at this location.
    cachedir: ''
# Specifies whether dnf should automatically expire metadata of repos, which are
# older than their corresponding configuration file (usually the dnf.conf file
# and the foo.repo file). Default is true (perform the check). Expire of
# metadata is also affected by metadata age. See also 'metadata_expire'.
    check_config_file_age: 'true'
# Remove dependencies that are no longer used during dnf remove. A package only
# qualifies for removal via 'clean_requirements_on_remove' if it was installed
# through DNF but not on explicit user request, i.e. it was pulled in as a
# dependency. The default is true ('installonlypkgs' are never automatically
# removed).
    clean_requirements_on_remove: 'true'
# Path to the default main configuration file. Default is '/etc/dnf/dnf.conf'.
    config_file_path: '/etc/dnf/dnf.conf'
# Debug messages output level, in the range 0 to 10. The higher the number the
# more debug output is put to stdout. Default is '2'.
    debuglevel: '2'
# If enabled the default answer to user confirmation prompts will be 'yes'. Not
# to be confused with 'assumeyes' which will not prompt at all.
# Default is false.
    defaultyes: 'false'
# Error messages output level, in the range 0 to 10. The higher the number the
# more error output is put to stderr. Default is 3. This is deprecated in DNF
# and overwritten by --verbose commandline option.
    errorlevel: '3'
# Should the dnf client exit immediately when something else has the lock.
# Default is false.
    exit_on_lock: 'false'
# Should the dnf attempt to automatically verify GPG verification keys using
# the DNS system. This option requires libunbound to be installed on the client
# system. This system has two main features. The first one is to check if any
# of the already installed keys have been revoked. Automatic removal of the key
# is not yet available, so it is up to the user, to remove revoked keys from
# the system. The second feature is automatic verification of new keys when a
# repository is added to the system. In interactive mode, the result is written
# to the output as a suggestion to the user. In non-interactive mode (i.e. when
# '-y' is used), this system will automatically accept keys that are available
# in the DNS and are correctly signed using DNSSEC. It will also accept keys
# that do not exist in the DNS system and their NON-existence is
# cryptographically proven using DNSSEC. This is mainly to preserve backward
# compatibility.
    gpgkey_dns_verification: ''
# List of the following: 'optional', 'default', 'mandatory'. Tells dnf which
# type of packages in groups will be installed when 'groupinstall' is called.
    group_package_types:
    - 'default'
    - 'mandatory'
# If set to 'true' (default is 'false'), RPM will allow attempts to install
# packages incompatible with the CPU's architecture. Often used together with
# 'arch' option.
    ignorearch: 'false'
# When this option is set to 'true' and a new package is about to be installed,
# all packages linked by weak dependency relation (Recommends or Supplements
# flags) with this package will pulled into the transaction. Default is 'true'.
    install_weak_deps: 'true'
# List of provide names of packages that should only ever be installed, never
# upgraded. Kernels in particular fall into this category. These packages are
# never removed by 'dnf autoremove' even if they were installed as dependencies
# (see 'clean_requirements_on_remove' for auto removal details). This option
# append the list values to the default 'installonlypkgs' list used by DNF. The
# number of kept package versions is regulated by installonly_limit.
    installonlypkgs: ''
# Number of 'installonly' packages allowed to be installed concurrently.
# Defaults to '3'. The minimal number of 'installonly' packages is '2'. Value
# '0' or '1' means unlimited number of 'installonly' packages.
    installonly_limit: '3'
# Keeps downloaded packages in the cache when set to 'true'. Even if it is set
# to 'false' and packages have not been installed they will still persist until
# next successful transaction. The default is false.
    keepcache: 'false'
# Directory where the log files will be stored. Default is '/var/log'.
    logdir: '/var/log'
# Log files are rotated when they grow bigger than 'log_size' bytes. If
# 'log_size' is '0', the rotation is not performed. The default is '1 MB'. Valid
# units are 'k', 'M', 'G'. The size applies for individual log files, not the
# sum of all log files. See also 'log_rotate'.
    log_size: '1 MB'
# Log files are rotated 'log_rotate' times before being removed. If 'log_rotate'
# is '0', the rotation is not performed. Default is '4'.
    log_rotate: '4'
# The minimal period between two consecutive makecache timer runs. The command
# will stop immediately if it's less than this time period since its last run.
# Does not affect simple makecache run. Use '0' to completely disable automatic
# metadata synchronizing. The default corresponds to three hours. The value is
# rounded to the next commenced hour.
    metadata_timer_sync: ''
# Set this to '$name:$stream' to override PLATFORM_ID detected from
# /etc/os-release. It is necessary to perform a system upgrade and switch to a
# new platform.
    module_platform_id: ''
# This option only has affect during an install/update. It enables dnf's
# obsoletes processing logic, which means it makes dnf check whether any
# dependencies of given package are no longer required and removes them. Useful
# when doing distribution level upgrades. Default is 'true'.
    obsoletes: 'true'
# List of directories that are searched for plugin configurations to load. All
# configuration files found in these directories, that are named same as a
# plugin, are parsed. The default path is '/etc/dnf/plugins'.
    pluginconfpath: '/etc/dnf/plugins'
# List of directories that are searched for plugins to load. Plugins found in
# any of the directories in this configuration option are used. The default
# contains a Python version-specific path.
    pluginpath: ''
# List of packages that DNF should never completely remove. They are protected
# via 'obsoletes' as well as user/plugin removals. DNF will protect also the
# package corresponding to the running version of the kernel.
    protected_packages:
    - 'dnf'
    - 'glob:/etc/yum/protected.d/*.conf'
    - 'glob:/etc/dnf/protected.d/*.conf'
# DNF searches for repository configuration files in the paths specified by
# 'reposdir'. The behavior of 'reposdir' could differ when it is used along with
# '--installroot' option.
    reposdir: ''
# RPM debug scriptlet output level. One of: 'critical', 'emergency', 'error',
# 'warn', 'info' or 'debug'. Default is 'info'.
    rpmverbosity: 'info'
# List of strings adding extra flags for the RPM transaction.
    tsflags:
    - 'noscripts'
    - 'test'
    - 'notriggers'
    - 'nodocs'
    - 'justdb'
    - 'nocontexts'
    - 'nocaps'
    - 'nocrypto'
# Set this to 'false' to disable the automatic running of group upgrade when
# running the upgrade command. Default is 'true' (perform the operation).
    upgrade_group_objects_upgrade: 'true'
# Total bandwidth available for downloading. Meaningful when used with the
# 'throttle' option. Storage size is in bytes by default but can be specified
# with a unit of storage. Valid units are 'k', 'M', 'G'.
    bandwidth: 'true'
# When enabled (the default), DNF will save bandwidth by downloading much
# smaller delta RPM files, rebuilding them to RPM locally. However, this is
# quite CPU and I/O intensive.
    deltarpm: 'true'
# When the relative size of delta vs pkg is larger than this, delta is not used.
# Default value is 75 (deltas must be at least 25% smaller than the pkg). Use
# '0' to turn off delta rpm processing. Local repositories (with file://
# baseurl) have delta rpms turned off by default.
    deltarpm_percentage: '75'
# Determines whether DNF will allow the use of package groups for this
# repository. Default is 'true' (package groups are allowed).
    enablegroups: 'true'
# Exclude packages of this repository, specified by a name or a glob and
# separated by a comma, from all operations. Can be disabled using
# '--disableexcludes' command line switch.
    excludepkgs: ''
# If enabled a metric is used to find the fastest available mirror. This
# overrides the order provided by the mirrorlist/metalink file itself. This
# file is often dynamically generated by the server to provide the best
# download speeds and enabling 'fastestmirror' overrides this. The default is
# 'false'.
    fastestmirror: 'false'
# Whether to perform GPG signature check on packages found in this repository.
# The default is 'false'. This option can only be used to strengthen the active
# RPM security policy set with the '%_pkgverify_level' macro. That means, if the
# macro is set to 'signature' or 'all' and this option is 'false', it will be
# overridden to 'true' during DNF runtime, and a warning will be printed. To
# squelch the warning, make sure this option is 'true' for every enabled
# repository, and also enable 'localpkg_gpgcheck'.
    gpgcheck: 'false'
# Include packages of this repository, specified by a name or a glob and
# separated by a comma, in all operations. Inverse of 'excludepkgs', DNF will
# exclude any package in the repository that doesn't match this list. This works
# in conjunction with 'exclude' and doesn't override it.
    includepkgs: ''
# Determines how DNF resolves host names. Set this to '4'/'IPv4' or '6'/'IPv6'
# to resolve to IPv4 or IPv6 addresses only. By default, DNF resolves to either
# addresses.
    ip_resolve: ''
# Whether to perform a GPG signature check on local packages (packages in a
# file, not in a repository). The default is 'false'. This option is subject to
# the active RPM security policy (see gpgcheck for more details).
    localpkg_gpgcheck: 'false'
# Maximum number of simultaneous package downloads. Defaults to '3'.
    max_parallel_downloads: '3'
# The period after which the remote repository is checked for metadata update
# and in the positive case the local metadata cache is updated. The default
# corresponds to 48 hours. Set this to '-1' or never to make the repo never
# considered expired. Expire of metadata can bee also triggered by change of
# timestamp of configuration files (dnf.conf, <repo>.repo). See also
# 'check_config_file_age'.
    metadata_expire: ''
# This sets the low speed threshold in bytes per second. If the server is
# sending data at the same or slower speed than this value for at least timeout
# option seconds, DNF aborts the connection. The default is '1000'. Valid units
# are 'k', 'M', 'G'.
    minrate: '1000'
# URL of a proxy server to connect through. If None is specified then direct
# connection is used (the default).
    proxy: ''
# The username to use for connecting to the proxy server. Empty by default.
    proxy_username: ''
# The password to use for connecting to the proxy server. Empty by default.
    proxy_password: ''
# The authentication method used by the proxy server. Valid values are 'basic',
# 'digest', 'negotiate', 'ntlm', 'digest_ie', 'ntlm_wb', 'none' and 'any'
# (default).
    proxy_auth_method: ''
# Whether to perform GPG signature check on this repository's metadata. The
# default is 'false'.
    repo_gpgcheck: ''
# Set the number of times any attempt to retrieve a file should retry before
# returning an error. Setting this to '0' makes dnf try forever. Default is '10'.
    retries: '10'
# If enabled, DNF will continue running and disable the repository that couldn't
# be synchronized for any reason. This option doesn't affect skipping of
# unavailable packages after dependency resolution. To check inaccessibility of
# repository use in in combination with refresh command line option. The default
# is 'false'.
    skip_if_unavailable: 'false'
# Path to the directory or file containing the certificate authorities to
# verify SSL certificates. Empty by default - uses system default.
    sslcacert: ''
# When enabled (the default), remote SSL connections are verified. If the client
# can not be authenticated connecting fails and the given repo is not used
# further. On False, SSL connections can be used but are not verified.
    sslverify: 'true'
# Path to the SSL client certificate used to connect to remote sites. Empty by
# default.
    sslclientcert: ''
# Path to the SSL client key used to connect to remote sites. Empty by default.
    sslclientkey: ''
# Limits the downloading speed. It might be an absolute value or a percentage,
# relative to the value of the bandwidth option option. '0' means no throttling
# (the default). The absolute value is in bytes by default but can be specified
# with a unit of storage. Valid units are 'k', 'M', 'G'.
    throttle: '0'
# Number of seconds to wait for a connection before timing out. Used in
# combination with minrate option option. Defaults to '30' seconds.
    timeout: '30'
# The username to use for connecting to repo with basic HTTP authentication.
# Empty by default.
    username: ''
# The password to use for connecting to repo with basic HTTP authentication.
# Empty by default.
    password: ''
```
