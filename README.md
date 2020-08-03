# BIND Module

## Overview

This module install and configure bind dns server.

## Usage

Default configuration:

```puppet
include bind
```

Change configuration file settings:

```puppet
class { 'bind':
    listen_on              => 'port 53 { 127.0.0.1; }',
    listen_on_v6           => 'port 53 { ::1; }',
    directory              => '"/var/named"',
    dump_file              => '"/var/named/data/cache_dump.db"',
    statistics_file        => '"/var/named/data/named_stats.txt"',
    memstatistics_file     => '"/var/named/data/named_mem_stats.txt"',
    allow_query            => '{ localhost; }',
    allow_update           => '{ none; }',
    allow_transfer         => '{ none; }',
    recursion              => 'no',
    dnssec_enable          => 'yes',
    dnssec_validation      => 'yes',
    bindkeys_file          => '"/etc/named.iscdlv.key"',
    managed_keys_directory => '"/var/named/dynamic"',
    pid_file               => '"/run/named/named.pid"',
    session_keyfile        => '"/run/named/session.key"',
    version                => '"[SECURED]"',
    server_id              => 'none',
    cleaning_interval      => '120',
    interface_interval     => '0',
    max_ncache_ttl         => '3600',
    nnotify                => 'no',
    logging                => true,
    filter_aaaa_on_v4      => 'no',
    zone                   => {
      'example.com' => [
        'type master',
        'file "example.com.db"',
        'allow-transfer { none; }',
        'allow-query    { any; }',
        'allow-update   { none; }',
      ],
    },
    include                => [ '"/etc/named.rfc1912.zones"', '"/etc/named.root.key"' ],
}
```

Create zone file:

```puppet
bind::zone_file { 'example.com.db':
    file_name       => 'example.com.db',
    nameserver      => 'ns1.example.com.',
    admin           => 'admin@example.com.',
    ttl             => '3600',
    serial          => '1',
    refresh         => '3600',
    retry           => '1800',
    expire          => '3600',
    minimum         => '3600',
    records         => [
'@      IN      NS      ns1.example.com.',
'@      IN      A       192.168.1.105',
'ns1    IN      A       192.168.1.105',
'www    IN      A       192.168.1.105'
    ],
}
```

