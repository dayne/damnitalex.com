# Standard library
require 'rake'
require 'yaml'
require 'fileutils'
require 'exifr'

# Load the configuration file
config = YAML.load_file("_config.yml")

desc "Default"
task :default do
	puts "\t rake image image=PATH_TO_ORIGINAL_IMAGE [title='something you want'] [blurb='a blurb']"
end

# rake image image=PATH_TO_ORIGINAL_IMAGE [title='something you want'] [blurb='a blurb']
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
 
	target = File.join("images",File.basename(image))
	if File.exists?(target)
		raise "This image already in: #{target}"
	end 

	convert="convert #{image} -resize '940x500>' -size 940x500 xc:black +swap -gravity center  -composite #{target}"
	system(convert)
 
	begin
  	exifr = EXIFR::JPEG.new(image)
  	date = exifr.date_time.strftime('%F')
	rescue 
		puts "blew up getting EXIFR"
		puts "setting time to now"
		date = Time.now.strftime('%F')
	end
	
  
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
image: #{target}
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
