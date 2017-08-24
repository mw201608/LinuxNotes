### Linux System Administration Common Commands

A list:
	1. To create a new user
	```
	sudo useradd new_user_name
	```
	2. To delete a user
	```
	sudo userdel username
	```
	3. To create a new user group
	```
	sudo groupadd group_name
	```
	4. To delete a user group
	```
	sudo groupdel group_name
	```
	5. To add a user to an existing group
	```
	sudo usermod -a -G group_name user_name
	```
	6. To list who are currently logged on
	```
	who
	```
	7. To list all the user accounts (including system/device service accounts which, by convention, have numeric user IDs in the low range, e.g. < 1000 or so)
	```
	cut -d: -f1 /etc/passwd
	```
	8. To check user and group numeric IDs
	```
	id user_name
	```
	...eg, print the numeric id of root: `id root`.

	9. To change the password for the current user
	```
	passwd
	```
	To change/set the password on behalf of a specific user under root account
	```
	passwd user_name
	```
	10. Find out the home directory of a user
	```
	grep username /etc/passwd
	```
	11. To stop all non-root users from login, create a file `/etc/nologin`
	```
	sudo touch /etc/nologin
	```
	However, users with ssh keys may still be able to login via key based PAM authentication despite the presence of file `/etc/nologin`. To stop key based login, we need to modify the file `/etc/pam.d/sshd` by changing the following line
	```
	auth required pam_nologin.so
	```
	to
	```
	account required pam_nologin.so
	```
	12. To set allowed users bypassing the file `/etc/nologin`, for example, if you want to allow users in the `sshusers` group to login via a text console, add the following line to file `/etc/pam.d/sshd` just before the line with `account required pam_nologin.so`:
	```
	account [success=1 default=ignore] pam_succeed_if.so quiet user ingroup sshusers
	```
	...In my case on a Ubuntu 16.04 system, the file `/etc/pam.d/sshd` now looks like
	```
	# Disallow non-root logins when /etc/nologin exists.
	account [success=1 default=ignore] pam_succeed_if.so quiet user ingroup sshusers
	account    required     pam_nologin.so
	```
