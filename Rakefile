# Standard library
require 'rake'
require 'yaml'
require 'fileutils'
require 'exifr'

# Load the configuration file
config = YAML.load_file("_config.yml")

# rake post["Post title"]
desc "Create a image post in _posts (rake image image='path/to/image' title='something' blurb='something else')"
task :image do
  image     = ENV['image']
  title     = ENV['title'] 
  blurb     = ENV['blurb']
  editor    = 'vim'
  
  # template  = config["post"]["template"]
  # extension = config["post"]["extension"]
  # editor    = config["editor"]

  if image.nil? or image.empty?
    raise "Please select an image for your post"
  end
  
  exifr = EXIFR::JPEG.new(image)
  date = exifr.date_time.strftime('%F')
  
  if title.nil? or title.empty?
    name = File.basename(image, '.*')
  else
    name = title
  end
  name = name.gsub(/(\'|\!|\?|\:|\s\z)/,"").gsub(/\s/,"-").downcase
  
  filename = "#{date}-#{name}.html"

  content = <<-EOF
---
title: #{title || 'Default title'}
image: #{image}
---
#{blurb || 'Default blurb'}
EOF
  
  if File.exists?("_posts/#{filename}")
    raise "The post already exists."
  else
    File.write("_posts/#{filename}", content)
    puts "#{filename} was created."
  
    if editor && !editor.nil?
      sleep 2
      system "#{editor} _posts/#{filename}"
    end
  end
end
