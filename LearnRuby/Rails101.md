#Rails101
####<font color='green'>增加数据库</font>
rails g migration add_user_id_to_group
rails g 数据库迁移  增加_ **user_id** _到_ **group**

```ruby
db/migrate/一串数字_add_user_id_to_group
class AddUserIdToGroup < ActiveRecord::Migration[5.0]
  def change
    add_column :groups, :user_id, :integer
  end
end
```
意思
add_column :groups, :user_id, :integer
增加_列    到groups， :user_id, :integer


```ruby
rake db:migrate
```
数据库迁移

```ruby
app/models/user.rb
class User < ApplicationRecord
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable

  has_many :groups
end
```
has_many :groups
拥有_许多 :groups
主表是user.rb
副表是groups.rb

解释：user拥有groups


```ruby
app/models/group.rb
class Group < ApplicationRecord
  belongs_to :user
  validates :title, presence: true
end
```
belongs_to :user
属于_to :user
我是user的人

####清数据库
```ruby
rails c
Group.delete_all
```

####建立post模型，数据库有content、group_id:integer user_id:integer
```ruby
rails g model post content:text group_id:integer user_id:integer
```

```ruby
config/routes.rb

Rails.application.routes.draw do
  devise_for :users
  resources :groups do
    resources :posts
  end
  root 'groups#index'
end
```



resources :groups do
  resources :posts
end

把groups属于主模块，下面有posts模型(rails g controller posts)


####重新载入routes
```ruby
rake routes
```


####增加分页功能
```ruby
gem "will_paginate"

def show
  @group = Group.find(params[:id])
  @posts = @group.posts.recent.paginate(:page => params[:page], :per_page => 5)
end
```
