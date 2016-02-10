# Notes in no particular order

242 - admin
255 - linux
222 - windows

```ruby
package 'httpd'  
```
installs latest version of that package

```ruby
service 'ntp' do
  action [ :enable, :start]
end
```
enabled on reboot and started

```ruby
file '/etc/motd' do
  content 'This computer is the property ...'
end
```
a file created with the content defined

```ruby 
 action: delete
```
will delete the file

'chef-apply' will run a chef command inline
'chef-apply --help' will send out the help block

