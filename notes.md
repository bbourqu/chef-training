# What I want to learn
* Staying OS Agnostic?
* Testing well


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

`chef-apply --help` will send out the help block  
`chef-apply -e` execute inline command  
`chef-apply <file>` run the recipe  

[File Resource Information](https://docs.chef.io/resource_file.html)

Declare out the actions that make sense eg. `action :create`

