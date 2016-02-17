namespace :assets do

  def invoke_or_reboot_rake_task(task)
    Rake::Task[task].invoke
  end

  task :test_node do
    begin
      `node -v`
    rescue Errno::ENOENT
      STDERR.puts <<-EOM
        Unable to find 'node' on the current path.
        EOM
      exit 1
    end
  end

  namespace :precompile do
    task :all => [#"assets:precompile:rjs",
                  "assets:precompile:less"]

    task :external => ["assets:test_node"] do
      Rake::Task["assets:precompile:all"].invoke
      `bundle install`
      `cp Gemfile Gemfile.txt`
      `cp Gemfile.lock Gemfile.lock.txt`
    end

    task :less do
      #parser = Less::Parser.new :filename => 'style.less'
      #tree = parser.parse
      #css = tree.to_cc
      #File.open('style.css', 'w') {|f| f.write(css) }
      
      
      #LessJs.compile File.read("style.less")
      
      #`npm install -g less`
      #unless $?.success?
      #  raise RuntimeError, "Failed to install less NPM."
      #end
      #`lessc style.less style.css`
      #unless $?.success?
      #  raise RuntimeError, "Failed to lessc."
      #end
    end

   #task :less do
   #  less = File.open('style.less', 'r').read
   #  parser = Less::Parser.new :paths => ['.']
   #  tree = parser.parse less
   #  css = tree.to_css
   ##Dir.mkdir 'public/css/'
   #  File.open('style.css', 'w') {|f| f.write(css) }
   #end
  end

  desc "Precompile JS and LESS assets"
  task :precompile do
    puts `bundle exec jekyll build`
  end

end
