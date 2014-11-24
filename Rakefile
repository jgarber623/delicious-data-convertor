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
  regex = /<DL><p>(.*?)\n<\/DL>/m
  input_file_contents = regex.match(File.read(input_filepath)).captures[0].gsub('</A>', '</A></DT>')
  output_file_contents = []

  Nokogiri::HTML.fragment(input_file_contents).css('dt').each do |dt|
    dd = dt.css('+ dd')
    link = dt.css('a')[0]

    out = {
      add_date: Time.at(link['add_date'].to_i),
      private: link['private'],
      tags: link['tags'].split(',').sort,
      title: link.content,
      url: link['href']
    }

    if dd.any?
      out[:comment] = dd[0].content.strip
    end

    output_file_contents.push(Hash[out.sort_by(&:first)].to_h)
  end

  output_file_contents.sort_by { |item| item[:add_date] }
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