$LOAD_PATH << "lib"
require 'i18n_data'

task :default => :spec
require 'spec/rake/spectask'
Spec::Rake::SpecTask.new {|t| t.spec_opts = ['--color']}

desc "write all languages to output"
task :all_languages do
  I18nData.languages.keys.each do |lc|
    `rake languages LANGUAGE=#{lc}`
  end
end

desc "write languages to output/languages_{language}"
task :languages do
  raise unless language = ENV['LANGUAGE']
  `mkdir output -p`
  data = I18nData.languages(language.upcase)
  File.open("output/languages_#{language.downcase}.yml",'w') {|f|f.puts data.to_yaml}
end

desc "write all countries to output"
task :all_countries do
  I18nData.languages.keys.each do |lc|
    `rake countries LANGUAGE=#{lc}`
  end
end

desc "write countries to output/countries_{language}"
task :countries do
  raise unless language = ENV['LANGUAGE']
  `mkdir output -p`
  data = I18nData.countries(language.upcase)
  File.open("output/countries_#{language.downcase}.yml",'w') {|f|f.puts data.to_yaml}
end

desc "write example output, just to show off :D"
task :example_output do
  `mkdir example_output -p`
  
  #all names for germany, france, united kingdom and unites states
  ['DE','FR','GB','US'].each do |cc|
    names = I18nData.languages.keys.map do |lc|
      begin
        [I18nData.countries(lc)[cc], I18nData.languages[lc]]
      rescue I18nData::NoTranslationAvailable
        nil
      end
    end
    File.open("example_output/all_names_for_#{cc}.txt",'w') do |f|
      f.puts names.reject(&:nil?).map{|x|x*" ---- "} * "\n"
    end
  end
end

desc "show stats"
task :stats do
  dir = "cache/file_data_provider"
  [:languages, :countries].each do |type|
    files = FileList["#{dir}/#{type}*"]
    lines = File.readlines(files.first).reject{|l|l.empty?}
    puts "#{lines.size} #{type} in #{files.size} languages"
  end
end

desc "write cache for I18nData::FileDataProvider"
task :write_cache_for_file_data_provider do
  require 'i18n_data/file_data_provider'
  require 'i18n_data/live_data_provider'
  I18nData::FileDataProvider.write_cache(I18nData::LiveDataProvider)
end

begin
  require 'jeweler'
  project_name = 'i18n_data'
  Jeweler::Tasks.new do |gem|
    gem.name = project_name
    gem.summary = "country/language names and 2-letter-code pairs, in 85 languages"
    gem.email = "grosser.michael@gmail.com"
    gem.homepage = "http://github.com/grosser/#{project_name}"
    gem.authors = ["Michael Grosser"]
    gem.add_dependency ['activesupport']
  end

  Jeweler::GemcutterTasks.new
rescue LoadError
  puts "Jeweler, or one of its dependencies, is not available. Install it with: sudo gem install technicalpickles-jeweler -s http://gems.github.com"
end