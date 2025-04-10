# Сборка RPM-пакета и создание репозитория
```
[root@localhost ~]#yum install -y wget rpmdevtools rpm-build createrepo yum-utils cmake gcc git nano

[root@localhost ~]# mkdir rpm && cd rpm
enabling appstream-source repository
enabling baseos-source repository
enabling extras-source repository
AlmaLinux 9 - AppStream - Source                                                                                                                     425 kB/s | 860 kB     00:02
AlmaLinux 9 - BaseOS - Source                                                                                                                        163 kB/s | 313 kB     00:01
AlmaLinux 9 - Extras - Source                                                                                                                        4.7 kB/s | 8.2 kB     00:01

nginx-1.20.1-20.el9.alma.1.src.rpm                                                                                                                   752 kB/s | 1.1 MB     00:01

[root@localhost rpm]# yumdownloader --source nginx

[root@localhost rpm]# rpm -Uvh nginx*.src.rpm

[root@localhost rpm]# yum-builddep nginx

[root@localhost rpm]# cd /root

[root@localhost ~]# git clone --recurse-submodules -j8 \
https://github.com/google/ngx_brotli
Cloning into 'ngx_brotli'...
remote: Enumerating objects: 237, done.
remote: Counting objects: 100% (37/37), done.
remote: Compressing objects: 100% (16/16), done.
remote: Total 237 (delta 24), reused 21 (delta 21), pack-reused 200 (from 1)
Receiving objects: 100% (237/237), 79.51 KiB | 370.00 KiB/s, done.
Resolving deltas: 100% (114/114), done.
Submodule 'deps/brotli' (https://github.com/google/brotli.git) registered for path 'deps/brotli'
Cloning into '/root/ngx_brotli/deps/brotli'...
remote: Enumerating objects: 7810, done.
remote: Counting objects: 100% (22/22), done.
remote: Compressing objects: 100% (21/21), done.
remote: Total 7810 (delta 7), reused 1 (delta 1), pack-reused 7788 (from 2)
Receiving objects: 100% (7810/7810), 40.62 MiB | 3.84 MiB/s, done.
Resolving deltas: 100% (5069/5069), done.
Submodule path 'deps/brotli': checked out 'ed738e842d2fbdf2d6459e39267a633c4a9b2f5d'

[root@localhost ~]# cd ngx_brotli/deps/brotli

[root@localhost brotli]# mkdir out && cd out

[root@localhost out]# cmake -DCMAKE_BUILD_TYPE=Release -DBUILD_SHARED_LIBS=OFF -DCMAKE_C_FLAGS="-Ofast -m64 -march=native -mtune=native -flto -funroll-loops -ffunction-sections -fdata-sections -Wl,--gc-sections" -DCMAKE_CXX_FLAGS="-Ofast -m64 -march=native -mtune=native -flto -funroll-loops -ffunction-sections -fdata-sections -Wl,--gc-sections" -DCMAKE_INSTALL_PREFIX=./installed ..
-- The C compiler identification is GNU 11.5.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Build type is 'Release'
-- Performing Test BROTLI_EMSCRIPTEN
-- Performing Test BROTLI_EMSCRIPTEN - Failed
-- Compiler is not EMSCRIPTEN
-- Looking for log2
-- Looking for log2 - not found
-- Looking for log2
-- Looking for log2 - found
-- Configuring done (2.8s)
-- Generating done (0.1s)
CMake Warning:
  Manually-specified variables were not used by the project:

    CMAKE_CXX_FLAGS


-- Build files have been written to: /root/ngx_brotli/deps/brotli/out

[root@localhost out]# cmake --build . --config Release -j 2 --target brotlienc
[  3%] Building C object CMakeFiles/brotlicommon.dir/c/common/constants.c.o
[  6%] Building C object CMakeFiles/brotlicommon.dir/c/common/context.c.o
[ 10%] Building C object CMakeFiles/brotlicommon.dir/c/common/dictionary.c.o
[ 13%] Building C object CMakeFiles/brotlicommon.dir/c/common/platform.c.o
[ 17%] Building C object CMakeFiles/brotlicommon.dir/c/common/shared_dictionary.c.o
[ 20%] Building C object CMakeFiles/brotlicommon.dir/c/common/transform.c.o
[ 24%] Linking C static library libbrotlicommon.a
[ 24%] Built target brotlicommon
[ 27%] Building C object CMakeFiles/brotlienc.dir/c/enc/backward_references.c.o
[ 31%] Building C object CMakeFiles/brotlienc.dir/c/enc/backward_references_hq.c.o
[ 34%] Building C object CMakeFiles/brotlienc.dir/c/enc/bit_cost.c.o
[ 37%] Building C object CMakeFiles/brotlienc.dir/c/enc/block_splitter.c.o
[ 41%] Building C object CMakeFiles/brotlienc.dir/c/enc/brotli_bit_stream.c.o
[ 44%] Building C object CMakeFiles/brotlienc.dir/c/enc/cluster.c.o
[ 48%] Building C object CMakeFiles/brotlienc.dir/c/enc/command.c.o
[ 51%] Building C object CMakeFiles/brotlienc.dir/c/enc/compound_dictionary.c.o
[ 55%] Building C object CMakeFiles/brotlienc.dir/c/enc/compress_fragment.c.o
[ 58%] Building C object CMakeFiles/brotlienc.dir/c/enc/compress_fragment_two_pass.c.o
[ 62%] Building C object CMakeFiles/brotlienc.dir/c/enc/dictionary_hash.c.o
[ 65%] Building C object CMakeFiles/brotlienc.dir/c/enc/encode.c.o
[ 68%] Building C object CMakeFiles/brotlienc.dir/c/enc/encoder_dict.c.o
[ 72%] Building C object CMakeFiles/brotlienc.dir/c/enc/entropy_encode.c.o
[ 75%] Building C object CMakeFiles/brotlienc.dir/c/enc/fast_log.c.o
[ 79%] Building C object CMakeFiles/brotlienc.dir/c/enc/histogram.c.o
[ 82%] Building C object CMakeFiles/brotlienc.dir/c/enc/literal_cost.c.o
[ 86%] Building C object CMakeFiles/brotlienc.dir/c/enc/memory.c.o
[ 89%] Building C object CMakeFiles/brotlienc.dir/c/enc/metablock.c.o
[ 93%] Building C object CMakeFiles/brotlienc.dir/c/enc/static_dict.c.o
[ 96%] Building C object CMakeFiles/brotlienc.dir/c/enc/utf8_util.c.o
[100%] Linking C static library libbrotlienc.a
[100%] Built target brotlienc

[root@localhost out]# cd ../../../..

[root@localhost out]# vi ~/rpmbuild/SPECS/nginx.spec


//
if ! ./configure \
    --prefix=%{_datadir}/nginx \
    --sbin-path=%{_sbindir}/nginx \
    --modules-path=%{nginx_moduledir} \
    --conf-path=%{_sysconfdir}/nginx/nginx.conf \
    --error-log-path=%{_localstatedir}/log/nginx/error.log \
    --http-log-path=%{_localstatedir}/log/nginx/access.log \
    --http-client-body-temp-path=%{_localstatedir}/lib/nginx/tmp/client_body \
    --http-proxy-temp-path=%{_localstatedir}/lib/nginx/tmp/proxy \
    --http-fastcgi-temp-path=%{_localstatedir}/lib/nginx/tmp/fastcgi \
    --http-uwsgi-temp-path=%{_localstatedir}/lib/nginx/tmp/uwsgi \
    --http-scgi-temp-path=%{_localstatedir}/lib/nginx/tmp/scgi \
    --pid-path=/run/nginx.pid \
    --lock-path=/run/lock/subsys/nginx \
    --user=%{nginx_user} \
    --group=%{nginx_user} \
    --with-compat \
    --with-debug \
    --add-module=/root/ngx_brotli \

//

[root@localhost ~]# cd ~/rpmbuild/SPECS/

[root@localhost ~]# rpmbuild -ba nginx.spec -D 'debug_package %{nil}'

[root@localhost ~]# ll ~/rpmbuild/RPMS/x86_64/
total 1988
-rw-r--r--. 1 root root   36243 Apr  7 23:07 nginx-1.20.1-20.el9.alma.1.x86_64.rpm
-rw-r--r--. 1 root root 1021059 Apr  7 23:07 nginx-core-1.20.1-20.el9.alma.1.x86_64.rpm
-rw-r--r--. 1 root root  759766 Apr  7 23:07 nginx-mod-devel-1.20.1-20.el9.alma.1.x86_64.rpm
-rw-r--r--. 1 root root   19366 Apr  7 23:07 nginx-mod-http-image-filter-1.20.1-20.el9.alma.1.x86_64.rpm
-rw-r--r--. 1 root root   31010 Apr  7 23:07 nginx-mod-http-perl-1.20.1-20.el9.alma.1.x86_64.rpm
-rw-r--r--. 1 root root   18174 Apr  7 23:07 nginx-mod-http-xslt-filter-1.20.1-20.el9.alma.1.x86_64.rpm
-rw-r--r--. 1 root root   53821 Apr  7 23:07 nginx-mod-mail-1.20.1-20.el9.alma.1.x86_64.rpm
-rw-r--r--. 1 root root   80438 Apr  7 23:07 nginx-mod-stream-1.20.1-20.el9.alma.1.x86_64.rpm

[root@localhost ~]# cp ~/rpmbuild/RPMS/noarch/* ~/rpmbuild/RPMS/x86_64/

[root@localhost ~]# cd ~/rpmbuild/RPMS/x86_64


[root@localhost x86_64]# yum localinstall *.rpm
Last metadata expiration check: 14:53:35 ago on Mon Apr  7 08:16:46 2025.
Dependencies resolved.
=====================================================================================================================================================================================
 Package                                              Architecture                    Version                                            Repository                             Size
=====================================================================================================================================================================================
Installing:
 nginx                                                x86_64                          2:1.20.1-20.el9.alma.1                             @commandline                           35 k
 nginx-all-modules                                    noarch                          2:1.20.1-20.el9.alma.1                             @commandline                          7.2 k
 nginx-core                                           x86_64                          2:1.20.1-20.el9.alma.1                             @commandline                          997 k
 nginx-filesystem                                     noarch                          2:1.20.1-20.el9.alma.1                             @commandline                          8.2 k
 nginx-mod-devel                                      x86_64                          2:1.20.1-20.el9.alma.1                             @commandline                          742 k
 nginx-mod-http-image-filter                          x86_64                          2:1.20.1-20.el9.alma.1                             @commandline                           19 k
 nginx-mod-http-perl                                  x86_64                          2:1.20.1-20.el9.alma.1                             @commandline                           30 k
 nginx-mod-http-xslt-filter                           x86_64                          2:1.20.1-20.el9.alma.1                             @commandline                           18 k
 nginx-mod-mail                                       x86_64                          2:1.20.1-20.el9.alma.1                             @commandline                           53 k
 nginx-mod-stream                                     x86_64                          2:1.20.1-20.el9.alma.1                             @commandline                           79 k
Installing dependencies:
 almalinux-logos-httpd                                noarch                          90.5.1-1.1.el9                                     appstream                              18 k

Transaction Summary
=====================================================================================================================================================================================
Install  11 Packages

Total size: 2.0 M
Total download size: 18 k
Installed size: 9.5 M
Is this ok [y/N]: y
Downloading Packages:
almalinux-logos-httpd-90.5.1-1.1.el9.noarch.rpm                                                                                                       16 kB/s |  18 kB     00:01
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                9.9 kB/s |  18 kB     00:01
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                             1/1
  Running scriptlet: nginx-filesystem-2:1.20.1-20.el9.alma.1.noarch                                                                                                             1/11
  Installing       : nginx-filesystem-2:1.20.1-20.el9.alma.1.noarch                                                                                                             1/11
  Installing       : nginx-core-2:1.20.1-20.el9.alma.1.x86_64                                                                                                                   2/11
  Installing       : almalinux-logos-httpd-90.5.1-1.1.el9.noarch                                                                                                                3/11
  Installing       : nginx-2:1.20.1-20.el9.alma.1.x86_64                                                                                                                        4/11
  Running scriptlet: nginx-2:1.20.1-20.el9.alma.1.x86_64                                                                                                                        4/11
  Installing       : nginx-mod-http-image-filter-2:1.20.1-20.el9.alma.1.x86_64                                                                                                  5/11
  Running scriptlet: nginx-mod-http-image-filter-2:1.20.1-20.el9.alma.1.x86_64                                                                                                  5/11
  Installing       : nginx-mod-http-perl-2:1.20.1-20.el9.alma.1.x86_64                                                                                                          6/11
  Running scriptlet: nginx-mod-http-perl-2:1.20.1-20.el9.alma.1.x86_64                                                                                                          6/11
  Installing       : nginx-mod-http-xslt-filter-2:1.20.1-20.el9.alma.1.x86_64                                                                                                   7/11
  Running scriptlet: nginx-mod-http-xslt-filter-2:1.20.1-20.el9.alma.1.x86_64                                                                                                   7/11
  Installing       : nginx-mod-mail-2:1.20.1-20.el9.alma.1.x86_64                                                                                                               8/11
  Running scriptlet: nginx-mod-mail-2:1.20.1-20.el9.alma.1.x86_64                                                                                                               8/11
  Installing       : nginx-mod-stream-2:1.20.1-20.el9.alma.1.x86_64                                                                                                             9/11
  Running scriptlet: nginx-mod-stream-2:1.20.1-20.el9.alma.1.x86_64                                                                                                             9/11
  Installing       : nginx-all-modules-2:1.20.1-20.el9.alma.1.noarch                                                                                                           10/11
  Installing       : nginx-mod-devel-2:1.20.1-20.el9.alma.1.x86_64                                                                                                             11/11
  Running scriptlet: nginx-mod-devel-2:1.20.1-20.el9.alma.1.x86_64                                                                                                             11/11
  Verifying        : almalinux-logos-httpd-90.5.1-1.1.el9.noarch                                                                                                                1/11
  Verifying        : nginx-2:1.20.1-20.el9.alma.1.x86_64                                                                                                                        2/11
  Verifying        : nginx-all-modules-2:1.20.1-20.el9.alma.1.noarch                                                                                                            3/11
  Verifying        : nginx-core-2:1.20.1-20.el9.alma.1.x86_64                                                                                                                   4/11
  Verifying        : nginx-filesystem-2:1.20.1-20.el9.alma.1.noarch                                                                                                             5/11
  Verifying        : nginx-mod-devel-2:1.20.1-20.el9.alma.1.x86_64                                                                                                              6/11
  Verifying        : nginx-mod-http-image-filter-2:1.20.1-20.el9.alma.1.x86_64                                                                                                  7/11
  Verifying        : nginx-mod-http-perl-2:1.20.1-20.el9.alma.1.x86_64                                                                                                          8/11
  Verifying        : nginx-mod-http-xslt-filter-2:1.20.1-20.el9.alma.1.x86_64                                                                                                   9/11
  Verifying        : nginx-mod-mail-2:1.20.1-20.el9.alma.1.x86_64                                                                                                              10/11
  Verifying        : nginx-mod-stream-2:1.20.1-20.el9.alma.1.x86_64                                                                                                            11/11

Installed:
  almalinux-logos-httpd-90.5.1-1.1.el9.noarch                    nginx-2:1.20.1-20.el9.alma.1.x86_64                    nginx-all-modules-2:1.20.1-20.el9.alma.1.noarch
  nginx-core-2:1.20.1-20.el9.alma.1.x86_64                       nginx-filesystem-2:1.20.1-20.el9.alma.1.noarch         nginx-mod-devel-2:1.20.1-20.el9.alma.1.x86_64
  nginx-mod-http-image-filter-2:1.20.1-20.el9.alma.1.x86_64      nginx-mod-http-perl-2:1.20.1-20.el9.alma.1.x86_64      nginx-mod-http-xslt-filter-2:1.20.1-20.el9.alma.1.x86_64
  nginx-mod-mail-2:1.20.1-20.el9.alma.1.x86_64                   nginx-mod-stream-2:1.20.1-20.el9.alma.1.x86_64

Complete!

[root@localhost x86_64]# systemctl start nginx

[root@localhost x86_64]# systemctl status nginx
● nginx.service - The nginx HTTP and reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; preset: disabled)
     Active: active (running) since Mon 2025-04-07 23:11:30 EDT; 7s ago
    Process: 48266 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)
    Process: 48267 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
    Process: 48268 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
   Main PID: 48269 (nginx)
      Tasks: 2 (limit: 11084)
     Memory: 3.7M
        CPU: 134ms
     CGroup: /system.slice/nginx.service
             ├─48269 "nginx: master process /usr/sbin/nginx"
             └─48270 "nginx: worker process"

Apr 07 23:11:30 localhost.localdomain systemd[1]: Starting The nginx HTTP and reverse proxy server...
Apr 07 23:11:30 localhost.localdomain nginx[48267]: nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
Apr 07 23:11:30 localhost.localdomain nginx[48267]: nginx: configuration file /etc/nginx/nginx.conf test is successful
Apr 07 23:11:30 localhost.localdomain systemd[1]: Started The nginx HTTP and reverse proxy server.

[root@localhost x86_64]# mkdir /usr/share/nginx/html/repo

[root@localhost x86_64]# cp ~/rpmbuild/RPMS/x86_64/*.rpm /usr/share/nginx/html/repo/

[root@localhost x86_64]# createrepo /usr/share/nginx/html/repo/
Directory walk started
Directory walk done - 10 packages
Temporary output repo path: /usr/share/nginx/html/repo/.repodata/
Preparing sqlite DBs
Pool started (with 5 workers)
Pool finished

[root@localhost x86_64]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful

[root@localhost x86_64]# nginx -s reload

[root@localhost x86_64]# curl -a http://localhost/repo/
<html>
<head><title>Index of /repo/</title></head>
<body>
<h1>Index of /repo/</h1><hr><pre><a href="../">../</a>
<a href="repodata/">repodata/</a>                                          08-Apr-2025 03:12                   -
<a href="nginx-1.20.1-20.el9.alma.1.x86_64.rpm">nginx-1.20.1-20.el9.alma.1.x86_64.rpm</a>              08-Apr-2025 03:12               36243
<a href="nginx-all-modules-1.20.1-20.el9.alma.1.noarch.rpm">nginx-all-modules-1.20.1-20.el9.alma.1.noarch.rpm</a>  08-Apr-2025 03:12                7357
<a href="nginx-core-1.20.1-20.el9.alma.1.x86_64.rpm">nginx-core-1.20.1-20.el9.alma.1.x86_64.rpm</a>         08-Apr-2025 03:12             1021059
<a href="nginx-filesystem-1.20.1-20.el9.alma.1.noarch.rpm">nginx-filesystem-1.20.1-20.el9.alma.1.noarch.rpm</a>   08-Apr-2025 03:12                8442
<a href="nginx-mod-devel-1.20.1-20.el9.alma.1.x86_64.rpm">nginx-mod-devel-1.20.1-20.el9.alma.1.x86_64.rpm</a>    08-Apr-2025 03:12              759766
<a href="nginx-mod-http-image-filter-1.20.1-20.el9.alma.1.x86_64.rpm">nginx-mod-http-image-filter-1.20.1-20.el9.alma...&gt;</a> 08-Apr-2025 03:12               19366
<a href="nginx-mod-http-perl-1.20.1-20.el9.alma.1.x86_64.rpm">nginx-mod-http-perl-1.20.1-20.el9.alma.1.x86_64..&gt;</a> 08-Apr-2025 03:12               31010
<a href="nginx-mod-http-xslt-filter-1.20.1-20.el9.alma.1.x86_64.rpm">nginx-mod-http-xslt-filter-1.20.1-20.el9.alma.1..&gt;</a> 08-Apr-2025 03:12               18174
<a href="nginx-mod-mail-1.20.1-20.el9.alma.1.x86_64.rpm">nginx-mod-mail-1.20.1-20.el9.alma.1.x86_64.rpm</a>     08-Apr-2025 03:12               53821
<a href="nginx-mod-stream-1.20.1-20.el9.alma.1.x86_64.rpm">nginx-mod-stream-1.20.1-20.el9.alma.1.x86_64.rpm</a>   08-Apr-2025 03:12               80438
</pre><hr></body>
</html>

[root@localhost x86_64]# cat >> /etc/yum.repos.d/otus.repo << EOF
[otus]
name=otus-linux
baseurl=http://localhost/repo
gpgcheck=0
enabled=1
EOF

[root@localhost x86_64]# yum repolist enabled | grep otus
otus                             otus-linux

[root@localhost x86_64]# cd /usr/share/nginx/html/repo/

[root@localhost repo]# wget https://repo.percona.com/yum/percona-release-latest.noarch.rpm
--2025-04-07 23:18:46--  https://repo.percona.com/yum/percona-release-latest.noarch.rpm
Resolving repo.percona.com (repo.percona.com)... 49.12.125.205, 2a01:4f8:242:5792::2
Connecting to repo.percona.com (repo.percona.com)|49.12.125.205|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 28300 (28K) [application/x-redhat-package-manager]
Saving to: ‘percona-release-latest.noarch.rpm’

percona-release-latest.noarch.rpm             100%[==============================================================================================>]  27.64K  --.-KB/s    in 0s

2025-04-07 23:18:47 (208 MB/s) - ‘percona-release-latest.noarch.rpm’ saved [28300/28300]

[root@localhost repo]# createrepo /usr/share/nginx/html/repo/
Directory walk started
Directory walk done - 11 packages
Temporary output repo path: /usr/share/nginx/html/repo/.repodata/
Preparing sqlite DBs
Pool started (with 5 workers)
Pool finished

[root@localhost repo]# yum makecache
AlmaLinux 9 - AppStream                                                                                                                              2.2 kB/s | 4.2 kB     00:01
AlmaLinux 9 - AppStream                                                                                                                              2.7 MB/s |  15 MB     00:05
AlmaLinux 9 - BaseOS                                                                                                                                 2.0 kB/s | 3.8 kB     00:01
AlmaLinux 9 - BaseOS                                                                                                                                 2.7 MB/s |  18 MB     00:06
AlmaLinux 9 - Extras                                                                                                                                 2.1 kB/s | 3.3 kB     00:01
AlmaLinux 9 - Extras                                                                                                                                 5.2 kB/s |  13 kB     00:02
otus-linux                                                                                                                                           186 kB/s | 7.2 kB     00:00
Metadata cache created.

[root@localhost repo]# yum list | grep otus
percona-release.noarch                               1.0-30                              otus

[root@localhost repo]# yum install -y percona-release.noarch
Last metadata expiration check: 0:01:16 ago on Mon Apr  7 23:19:49 2025.
Dependencies resolved.
=====================================================================================================================================================================================
 Package                                           Architecture                             Version                                     Repository                              Size
=====================================================================================================================================================================================
Installing:
 percona-release                                   noarch                                   1.0-30                                      otus                                    28 k

Transaction Summary
=====================================================================================================================================================================================
Install  1 Package

Total download size: 28 k
Installed size: 49 k
Downloading Packages:
percona-release-latest.noarch.rpm                                                                                                                    1.2 MB/s |  28 kB     00:00
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                1.1 MB/s |  28 kB     00:00
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                             1/1
  Installing       : percona-release-1.0-30.noarch                                                                                                                               1/1
  Running scriptlet: percona-release-1.0-30.noarch                                                                                                                               1/1
* Enabling the Percona Release repository
<*> All done!
* Enabling the Percona Telemetry repository
<*> All done!
* Enabling the PMM2 Client repository
<*> All done!
The percona-release package now contains a percona-release script that can enable additional repositories for our newer products.

Note: currently there are no repositories that contain Percona products or distributions enabled. We recommend you to enable Percona Distribution repositories instead of individual product repositories, because with the Distribution you will get not only the database itself but also a set of other componets that will help you work with your database.

For example, to enable the Percona Distribution for MySQL 8.0 repository use:

  percona-release setup pdps8.0

Note: To avoid conflicts with older product versions, the percona-release setup command may disable our original repository for some products.

For more information, please visit:
  https://docs.percona.com/percona-software-repositories/percona-release.html


  Verifying        : percona-release-1.0-30.noarch                                                                                                                               1/1

Installed:
  percona-release-1.0-30.noarch

Complete!
```
