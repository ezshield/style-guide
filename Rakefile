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
    task :all => ["assets:precompile:rjs",
                  "assets:precompile:less"]

    task :external => ["assets:test_node"] do
      Rake::Task["assets:precompile:all"].invoke
    end

    task :rjs do
      `cd public/js; ../../bin/node lib/r.js -o build.js`
      unless $?.success?
        raise RuntimeError, "JS compilation with r.js failed."
      end
    end

    task :less do
      less = File.open('public/less/main.less', 'r').read
      parser = Less::Parser.new :paths => ['public/less/']
      tree = parser.parse less
      css = tree.to_css
      Dir.mkdir 'public/css/'
      File.open('public/css/styles.css', 'w') {|f| f.write(css) }
    end
  end

  desc "Precompile JS and LESS assets"
  task :precompile do
    invoke_or_reboot_rake_task "assets:precompile:all"
    puts `bundle exec jekyll build`
  end

end
