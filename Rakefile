require 'rake/clean'
require 'rake/phony'
# require 'rake/ctools'
require 'rake'
require 'date'

require 'optparse'

# default task is show posts lastest
task default: ['posts:lastest']

task posts: ['posts:lastest']

desc 'a name args '
task :name, [:first_name, :last_name] do |_t, args|
  puts "#{_t},#{args.first_name}"
  args.with_defaults(first_name: 'John', last_name: 'Dough')
  puts "First name is #{args.first_name}"
  puts "Last  name is #{args.last_name}"
end




desc 'Method #3: Use ARGV to add two numbers and log the result'
task :add_m1 do

  ARGV.each { |a| task a.to_sym do ; end }

  puts ARGV[1].to_i + ARGV[2].to_i

end

desc 'Method #4: Use OptionParser to add two numbers and log the result'
task :add, [:num1, :num] do |_t, args|
  options = {}
  # opts = OptionParser.new
  # opts.banner = 'Usage: rake add [options]'
  # opts.on('-o', '--one ARG', Integer) { |num1| options[:num1] = num1 }
  # opts.on('-t', '--two ARG', Integer) { |num2| options[:num2] = num2 }

  # args = opts.order!(ARGV) {}

  # puts ARGV
  # opts.parse!(args)

  # puts options

  # puts options[:num1].to_i + options[:num2].to_i
  # exit

  opts = OptionParser.new do |op|
    op.banner = "Usage: rake add_numbers [OPTIONS]"
    op.on('-o', '--one ARG', Integer, 'First number') { |num| options[:num1] = num }
    op.on('-t', '--two ARG', Integer, 'Second number') { |num| options[:num2] = num }
  end

  opts.parse!(ARGV)

  raise OptionParser::MissingArgument if %i[num1 num2].any? { |key| options[key].nil? }

  puts options[:num1].to_i + options[:num2].to_i

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
    sh "code _drafts/#{draft_name}.md"
  end

  desc 'Creat a blog drafts'
  task :rename do |_t|
    last = Dir['_drafts/*.md']

    last.each do| f|
      num = f.split('-')[1].to_i - 1

      draft_name = "drafts-#{num}"

      puts "rename #{f}  _drafts/#{draft_name}.md"

      # sh "mv #{f}  _drafts/#{draft_name}.md"

    end

  end

end

namespace :posts do
  # posts file format :yyyy-mm-dd-#{notes_name}_#{num}
  # weekly notes: weekly
  # yyyy-mm-dd-#{notes_name}_notes_#{num}
  #
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

  desc 'Show a next Blog posts, default :notes_name is "weekly notes"'
  task :next, [:notes_name] do |_t, args|
    args.with_defaults(notes_name: 'weekly')

    name = args.notes_name

    if name.eql?('weekly')
      last_name = weekly_notes_name
    elsif name.eql?('read')
      last_name = read_notes_name
    else
      puts 'not support'
    end

    puts "next:#{last_name}"
  end

  desc 'Create a Blog posts,this is :post alias .'
  task new: :post

  desc 'Create a Blog posts, weekly notes.'
  task :post do |_t|
    name = weekly_notes_name

    puts "create posts #{name}"

    notes = File.new("_posts/#{name}", 'w')

    notes.puts weekly_notes_head

    notes.close
  end

  desc 'Create a Blog posts from a draft .'
  task :draft do |_t|

    draft_name = Dir['_drafts/*.md'].max

    draft_notes = open(draft_name)

    # 默认第1行为标题。 读取标题
    title = draft_notes.readline
    draft_title = title.delete('#').strip

    puts "title: #{draft_title}"

    notes_head = weekly_notes_head.gsub('NOTES_TITLE_TODO_REPALCE', draft_title)

    # puts notes_head

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

    puts "creat posts #{name} from draft #{draft_name}"

    sh "code _posts/#{name}"

  end

  desc 'Create a Blog posts, read notes.'
  task :read do |_t|
    name = read_notes_name

    puts "create posts #{name}"

    notes = File.new("_posts/#{name}", 'w')

    notes.puts read_notes_head

    notes.close

    sh "code _posts/#{name}"
  end

  namespace :read do


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

def read_notes_name
  day = Date.today

  last_name = Dir['_posts/*-read-*.md'].max

  year = last_name.split('-')[5].to_i

  num = last_name.split('-')[6].to_i + 1

  name = if num < 100
           "#{day}-read-notes-#{year}-#{'%02d' % num}.md"
         else
           "#{day}-read-notes-#{year + 1}-#{format('%02d', 1)}.md"
         end
  name
end

def read_notes_head
  templete_head = %(---
layout: post
title: "NOTES_TITLE_TODO_REPALCE"
description: "NOTES_TITLE_TODO_REPALCE"
author: "魏仁言"
date:  #{Time.now}
categories: read notes
toc: true
---
)

  templete_head
end
