# What I want to learn
* Staying OS Agnostic?
* Testing well


# Notes in no particular order

232 - admin  
177 - linux  
146 - windows  

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

Declare out the attributes that make sense eg. `action :create`

---

`chef --help` will send out the help block
`chef generate <name>` generate a new app, cookbook, or component

[Chef Cookbook Information](https://docs.chef.io/cookbooks.html)

## Examples
`chef generate cookbook apache`  
`chef generate recipe server`  


---

Ensure you are `git`ing everything  
Read the `README.md` first  

---

`chef-apply` is for a one-off on recipes
`chef-client` is an agent that performs all the steps on a node rather than 'one-off'

```unix
chef-client --local-mode -r "recipe[apach::server]"
```
running the 'server' recipe from the 'apache' cookbook from the root dir of your cookbook repo

_NOTE:_  the chef-client assumes `/home/chef` unless you have a `cookbooks` directory

```unix
chef-client --local-mode -r "recipe[apach::server],recipe[workstation::setup]"
```
This command runs multiple recipes _NOTE:_ ensure there is no space around the comma(,)

---

[include_recipe](https://docs.chef.io/recipes.html#include-recipes)

```ruby
include_recipe 'workstation::setup'
```
runs the recipe here.  runs in the order provided if multiple `include_recipe`s are there





