require 'sequel'
def db_file
  File.expand_path '../Contents/Resources/docSet.dsidx', __FILE__
end

def db
  @db ||= Sequel.connect("sqlite://#{db_file}")
end

task db_file do
  db.run "CREATE TABLE searchIndex(id INTEGER PRIMARY KEY, name TEXT, type TEXT, path TEXT);"
  db.run "CREATE UNIQUE INDEX anchor ON searchIndex (name, type, path);"
end

task :init => db_file

task :clean do
  sh "rm -rf #{db_file}"
end

task :generate => :init do
  require 'nokogiri'
  require 'pathname'
  Dir["Contents/Resources/Documents/*.html"].each do |file|
    File.open(file) do |f|
      html = Nokogiri::HTML.parse(f.read)
      links = html.css "dl dt a"
      links.each do |l|
        name = l.parent.css("strong").text
        type = case name
                 when /^scm/ then "Function"
                 when /^SCM/ then "Constant"
                 else
                 "Procedure"
               end
        path = Pathname(file).basename.to_s +  "##{l['name']}"
        db.run "INSERT OR IGNORE INTO searchIndex(name, type, path) VALUES ('#{name}', '#{type}', '#{path}');"
      end
    end
  end
end
