# What I want to learn
* Staying OS Agnostic?
* Testing well


# Notes in no particular order

232 - node1  
177 - node3  
146 - node2  

```ruby
package 'httpd'  
```
installs latest version of that package

```ruby
service 'ntp' do
  action [ :enable, :start ]
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

```shell
chef-client --local-mode -r "recipe[apach::server]"
```
running the 'server' recipe from the 'apache' cookbook from the root dir of your cookbook repo

_NOTE:_  the chef-client assumes `/home/chef` unless you have a `cookbooks` directory

```shell
chef-client --local-mode -r "recipe[apach::server],recipe[workstation::setup]"
```
This command runs multiple recipes _NOTE:_ ensure there is no space around the comma(,)

---

[include_recipe](https://docs.chef.io/recipes.html#include-recipes)

```ruby
include_recipe 'workstation::setup'
```
runs the recipe here.  runs in the order provided if multiple `include_recipe`s are there

# Testing Cookbooks

* ChefDK
 * RuboCop
 * kitchen
 * rubocop
---

* Write test case first
* Run virtual machine and fail
* Write code
* Run virtual machine and succeed
* Destroy virtual machine

---

kitchen create -> kitchen converge -> kitchen verify -> kitchen destroy

```shell
kitchen init
```
```shell
kitchen converge [INSTANCE|REGEX|all]
```
_NOTE:_ converge doesn't kill the machine

```shell
kitchen verify [INSTANCE|REGEXP|all]
```
verifies the state of the machine after the cookbook runs

```shell
kitchen destroy
```
destroys a test instance

```shell
kitchen test
```

runs `kitchen destroy` -> `kitchen create` -> `kitchen converge` -> `kitchen verify` -> `kitchen destroy` in that order


[ServerSpec](http://serverspec.org)

## Example

```ruby
describe package('tree') do
  it { should be_installed }
end
```
_NOTE:_ there is a long directory where these specs live, see [Writing Test](http://kitchen.ci/docs/getting-started/writing-test)

---

# Capturing Information about the System

utilize ohai to gather information about your system as a [JSON](http://json.org) object

## Example
```ruby
#{node['ipaddress']}
```

## Templates

Use when we have formatted string interpolation in a file that will end up on the node

More information can be found at [Template Resource](https://docs.chef.io/resource_template.html)

found at `cookbooks/<cookbook>/templates/default/`

### Example of generating
```shell
chef generate template index.html
```
generates the file index.html.erb in `templates/default/` under the cookbook

`<%=` is known as a "angry squid/squid-rocket"

now from `#{node['ipaddress']}` to `<%= node['ipaddress'] %>` 


# Chef Server

1. Provision Server
2. Install Chef
3. Copy Web Server cookbook
4. Apply the cookbook

## Hosted Chef

[Sign up for an account](https://www.chef.io)

* Download Starter Kit
* Allow the Keys for your org to be reset

[Repo of all the cookbooks we need](https://github.com/chef-training/chef-essentials-repo)

`knife` - command-line tool...interfaces between local and Chef Server  

`berks` - command-line tool...uploads cookbooks to the Chef Server  

`knife bootstrap` - installs chef tools if they are not already installed; configures to communicate with the Chef Server; runs chef-client to apply a default run list

`knife node run_list add <node_name> "recipe[<some_recipe>]"` - add a recipe to the run list for a node

## Community Cookbooks

[Supermarket](https://supermarket.chef.io)

"Wrapper Cookbook" - add new attributes while keeping the core functionality of the original cookbook (Perferred)

```shell
╭─user@host  ~/chef-repo/cookbooks/myhaproxy ‹system› ‹master*›
╰─$ knife node show node1 -a ipaddress                                                                                                                                                       node1:
  ipaddress: ###.31.#.###
```
showing a node attribute example

## Roles

Describes a node to better define its use

```shell
knife node run_list set node2 "role[load_balancer]"
```
example of changing a node to a role

```shell
knife ssh "role:web" -x <user> -P <password> "sudo chef-client"
```
example of telling a group of nodes under the "web" role to run chef-client

## Search

[Chef Search](https://docs.chef.io/chef_search.html)

ohai is going to report back up to Chef Server

```ruby
all_web_nodes = search('node','role:web')
```
example of a search added to an attribute

## Environments

* Apply a node to an environment
* make search queries much more specific
* can define defferent functions of nodes that live on the same system
* `_default` is the default environment assigned to all nodes

_NOTE:_ exists under /environments under your root cookbook dir

```shell
knife node environment set <node> acceptance
```
Example of making a <node> be placed into a environment



