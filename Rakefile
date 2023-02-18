require 'rake/clean'
require 'rake/phony'
# require 'rake/ctools'
require 'rake'
require 'date'

# default task is show posts lastest
task default: ['posts:lastest']

task posts: ['posts:lastest']

desc 'a name args '
task :name, [:first_name, :last_name] do |_t, args|
  args.with_defaults(first_name: 'John', last_name: 'Dough')
  puts "First name is #{args.first_name}"
  puts "Last  name is #{args.last_name}"
end

namespace :drafts do
  desc 'Show  all blog drafts'
  task :all do |_t|
    Dir['_drafts/*.md'].each do |fn|
      puts fn.to_s
    end
  end

  desc 'Creat a blog drafts'
  task :new do |_t|
    last = Dir['_drafts/*.md'].max

    num = last.split('-')[1].to_i + 1

    draft_name = "drafts-#{num}"

    puts draft_name
    # open()
  end
end

namespace :posts do
  # posts file format :yyyy-mm-dd-#{notes_name}_#{num}
  # weekly notes: weekly
  # yyyy-mm-dd-#{notes_name}_notes_#{num}
  # read notes: read
  # yyyy-mm-dd-#{notes_name}_notes_#{yyyy}_#{num}
  #

  desc 'Show  all blog posts'
  task :all do |_t|
    Dir['_posts/*.md'].each do |fn|
      puts fn.to_s
    end
  end

  desc 'Show a lastest Blog posts, default :notes_name is "weekly notes"'
  task :lastest, [:notes_name] do |_t, args|
    args.with_defaults(notes_name: 'weekly')

    last_name = Dir["_posts/*-#{args.notes_name}-*.md"].max
    puts last_name.to_s
  end

  desc 'Create a Blog posts,this is :post alias .'
  task new: :post

  desc 'Create a Blog posts, weekly notes.'
  task :post do |t|
    now = Time.now
    day = Date.today
    num = 17
    puts "Date.new #{t.name},#{now},#{now.strftime('%Y-%m-%d')},#{weekly_notes_name}"

    puts weekly_notes_head.to_s

    name = weekly_notes_name

    puts name.to_s

    notes = File.new("_posts/#{name}", 'w')

    notes.puts weekly_notes_head

    notes.close
  end

  desc 'Create a Blog posts from a draft .'
  task :draft do |_t|
    Dir['_drafts/*.md'].each do |fn|
      puts fn.to_s
    end

    draft_name = Dir['_drafts/*.md'].max

    draft_notes = open(draft_name)

    # 默认第1行为标题。 读取标题
    title = draft_notes.readline
    draft_title = title.delete('#').strip

    puts draft_title

    notes_head = weekly_notes_head.gsub('NOTES_TITLE_TODO_REPALCE', draft_title)

    puts notes_head

    name = weekly_notes_name

    notes = File.new("_posts/#{name}", 'w')

    # 写入Posts Head
    notes.puts notes_head

    # 写入草稿内容
    notes.puts draft_notes.readlines

    # 关闭文件
    draft_notes.close
    notes.close

    # 删除草稿
    # File.delete(draft_notes)

    puts name.to_s
  end
end

def weekly_notes_name
  day = Date.today

  last_name = Dir['_posts/*-weekly-*.md'].max

  num = last_name.split('-')[5].to_i + 1

  name = "#{day}-weekly-notes-#{num}.md"

  name
end

def weekly_notes_head
  templete_head = %(---
layout: post
title: "NOTES_TITLE_TODO_REPALCE"
description: "NOTES_TITLE_TODO_REPALCE"
author: "魏仁言"
date:  #{Time.now}
categories: life notes
toc: true
---
)

  templete_head
end
