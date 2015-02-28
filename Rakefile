require 'json'
require 'yaml'

# Our custom YAML tags must retain their magic.
%w[ code ].each do |tag|
  YAML::add_builtin_type(tag) { |_,val| val.merge(:__tag__ => tag) }
end

desc 'Build all alternate versions of the specs.'
multitask :build => [ 'build:json' ]

namespace :build do
  note = 'Do not edit this file; changes belong in the appropriate YAML file.'

  desc 'Build JSON versions of the specs.'
  task :json do
    rm(Dir['specs/*.json'], :verbose => false)
    Dir.glob('specs/*.yml').each do |filename|
      json_file = filename.gsub('.yml', '.json')

      File.open(json_file, 'w') do |file|
        doc = YAML.load_file(filename).to_json()
        # Splice notice into the JSON text to ensure it is visible at the top
        doc.insert(1, "\"__ATTN__\":\"#{note}\",");
        file << doc
      end
    end
  end
end
