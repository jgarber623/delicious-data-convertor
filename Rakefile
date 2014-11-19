require 'rubygems'
require 'nokogiri'
require 'json'

def input_filepath
  'data/delicious.html'
end

def output_filepath(format)
  'data/delicious.' + format
end

def build_output_file_contents
  output_file_contents = []

  File.open(input_filepath, 'r') do |input_file|
    html = Nokogiri::HTML(input_file)

    html.css('dt').each do |dt|
      dd = dt.css('+ dd');
      link = dt.css('a')[0]

      out = {
        add_date: Time.at(link['add_date'].to_i),
        private: link['private'],
        tags: link['tags'].split(','),
        title: link.content,
        url: link['href']
      }

      if dd.any?
        out['comment'] = dd[0].content
      end

      output_file_contents.push(out)
    end
  end

  output_file_contents
end

namespace :convert do
  desc 'Convert data from `data/delicious.html` to JSON'
  task :json do
    File.open(output_filepath('json'), 'w') do |output_file|
      output_file.write(build_output_file_contents.to_json)
    end
  end
end

task :convert => 'convert:json'