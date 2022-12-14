# Getting Started

By default, pbench benchmarks collect configuration data of a system, stored in the sysinfo top level directory of a pbench result, collected at the beginning and end of a benchmark (with one exception, pbench-user-benchmark only collects at the end).

The structure of the sysinfo directory in a pbench result, for example,  `/var/lib/pbench-agent/pbench_userbenchmark_example_2019.07.18T12.00.00/` is given below followed by a brief explanation of each different type of configuration data.

* sysinfo
  * end
    * hostname
      * block-params.log
      * config-5.0.17-300.fc30.x86_64	 
      * libvirt/	 
      * lstopo.txt	 
      * security-mitigation-data.txt	 
      * sosreport-localhost-localhost-pbench-2019-06-10-mrqgzbh.tar.xz
      * sosreport-localhost-localhost-pbench-2019-06-10-mrqgzbh.tar.xz.md5	 

## config-[kernel_version]

The file contains kernel configuration data. 

The data is collected using [pbench-sysinfo-dump#L43](https://github.com/distributed-system-analysis/pbench/blob/main/agent/util-scripts/tool-meister/pbench-sysinfo-dump#L43). The script uses `uname` system utility (`systemcall` is a term used for all the APIs provided by the kernel) to collect kernel release information and then checks if a corresponding kernel configuration file exists on the system. If it does, the script simply copies the file, located in `/boot` directory, to the `sysinfo` directory.

The file contains data in a key value format where the key is a metric name and the value can be a string literal or a number. The keys and the values are separated by an equality sign.

## security-mitigation-data.txt

The file contains CPU vulnerabilities data and RHEL-specific flag settings. 

The data is collected using [pbench-sysinfo-dump#L50](https://github.com/distributed-system-analysis/pbench/blob/main/agent/util-scripts/tool-meister/pbench-sysinfo-dump#L50). The script checks if `/sys/devices/system/cpu/vulnerabilities` directory exists. If it does, the script prints the filenames and the contents of all the files located in the directory. After that, it repeats the same steps for the `/sys/kernel/debug/x86` directory.

The file contains data in a key value format where the key is a file name and the value is the content of the file.

## libvirt/

The directory provides information about libvirt, an open-source API, daemon and management tool for managing platform virtualization.

The data is collected using [pbench-sysinfo-dump#L67](https://github.com/distributed-system-analysis/pbench/blob/main/agent/util-scripts/tool-meister/pbench-sysinfo-dump#L67). The script copies libvirt files located at `/var/log/libvirt` and `/etc/libvirt` directories to the `sysinfo/libvirt/log` and `sysinfo/libvirt/etc` directories respectively. Only the files whose name follows the regex `*.log` are copied from the `/var/log/libvirt` directory.

## lstopo.txt

The file provides information about the topology of the system.

The data is collected using [pbench-sysinfo-dump#L77](https://github.com/distributed-system-analysis/pbench/blob/main/agent/util-scripts/tool-meister/pbench-sysinfo-dump#L77). The script executes the command `lstopo --of txt` and dumps its output into a text file only if `/usr/bin/lstopo` file exists on the system.

## block-params.log

The file provides information about block devices.

The data is collected using [pbench-sysinfo-dump#L84](https://github.com/distributed-system-analysis/pbench/blob/main/agent/util-scripts/tool-meister/pbench-sysinfo-dump#L84). The script loops over all the files which satisfy the regex: `/sys/block/[s,h,v]d\*[a-z]/` and prints each file name along with the contents of the file.

The file contains data in a key value format where the key is the file name and the value is the content of the file..

## sosreport tarball (e.g. sosreport-localhost-localhost-pbench-2019-05-29-rtvzlke.tar.xz)

The tarball contains system configuration and diagnostic information collected by invoking the `sosreport` command.

The data is collected using [pbench-sysinfo-dump#L91](https://github.com/distributed-system-analysis/pbench/blob/main/agent/util-scripts/tool-meister/pbench-sysinfo-dump#L91). The script uses `sosreport` command with different plugins to get the required system information. The resulting tarball contains a number of files copied from the system as well as the output of several commands executed on the system.

## insights tarball

The tarball contains system information gathered by the [insights-client](https://github.com/RedHatInsights/insights-client).

The data is collected using [pbench-sysinfo-dump#L188](https://github.com/distributed-system-analysis/pbench/blob/main/agent/util-scripts/tool-meister/pbench-sysinfo-dump#L188). The script uses `insights-client` command with different options to get the system information. The resulting tarball contains a number of files copied from the system as well as the output of several commands executed on the system.
