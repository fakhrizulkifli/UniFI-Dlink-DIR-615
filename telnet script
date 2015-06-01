#!/usr/bin/expect
set timeout 20
set ip [lindex $argv 0]
spawn telnet $ip 23
expect "login: "
send "Management\n"
expect "Password: "
send "TestingR2\n"
expect "# "
send "cat /etc/ppp/chap-secrets\n"
send "exit\n"
interact
