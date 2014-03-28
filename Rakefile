require "rubygems"

repo_url      = "git@github.com:TomRegan/tomregan.github.io.git"
public_dir    = "_site"
source_dir    = "."    # source file directory
deploy_dir    = "_deploy"
posts_dir     = "_posts"
drafts_dir    = "_drafts"
new_post_ext  = "md"
new_page_ext  = "md"
deploy_branch = "master"
# deploy_branch = "gh-pages"

#######################
# Working with Jekyll #
#######################

desc "Generate jekyll site"
task :build do
  puts "## Generating Site with Jekyll"
  system "jekyll build"
end

desc "preview the site in a web browser"
task :preview do
 puts "Starting to watch source with Jekyll."
  system "jekyll serve -w"
end

# usage rake new_post[my-new-post] or rake new_post['my new post'] or rake new_post (defaults to "new-post")
desc "Begin a new post in #{source_dir}/#{drafts_dir}"
task :new_post, :title do |t, args|
  title = args.title ? args.title : get_stdin("Enter a title for your post: ")
  filename = "#{source_dir}/#{drafts_dir}/#{Time.now.strftime('%Y-%m-%d')}-#{title}.#{new_post_ext}"
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end
  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "categories: "
    post.puts "---"
  end
end

# usage rake new_page[my-new-page] or rake new_page[my-new-page.html] or rake new_page (defaults to "new-page.markdown")
desc "Create a new page in #{source_dir}/(filename)/index.#{new_page_ext}"
task :new_page, :filename do |t, args|
  raise "### You haven't set anything up yet. First run `rake install` to set up an Octopress theme." unless File.directory?(source_dir)
  args.with_defaults(:filename => 'new-page')
  page_dir = [source_dir]
  if args.filename.downcase =~ /(^.+\/)?(.+)/
    filename, dot, extension = $2.rpartition('.').reject(&:empty?)         # Get filename and extension
    title = filename
    page_dir.concat($1.downcase.sub(/^\//, '').split('/')) unless $1.nil?  # Add path to page_dir Array
    if extension.nil?
      page_dir << filename
      filename = "index"
    end
    extension ||= new_page_ext
    page_dir = page_dir.map! { |d| d = d.to_url }.join('/')                # Sanitize path
    filename = filename.downcase.to_url

    mkdir_p page_dir
    file = "#{page_dir}/#{filename}.#{extension}"
    if File.exist?(file)
      abort("rake aborted!") if ask("#{file} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
    end
    puts "Creating new page: #{file}"
    open(file, 'w') do |page|
      page.puts "---"
      page.puts "layout: page"
      page.puts "title: \"#{title}\""
      page.puts "date: #{Time.now.strftime('%Y-%m-%d %H:%M')}"
      page.puts "comments: true"
      page.puts "sharing: true"
      page.puts "footer: true"
      page.puts "---"
    end
  else
    puts "Syntax error: #{args.filename} contains unsupported characters"
  end
end


desc "Clean out caches: .pygments-cache, .gist-cache, .sass-cache"
task :clean do
  rm_rf [".pygments-cache/**",
         ".gist-cache/**",
         ".sass-cache/**",
         "#{public_dir}/",
         "#{deploy_dir}/"
        ]
end

##############
# Deploying  #
##############

desc "deploy public directory to github pages"
multitask :publish do
  puts "## Deploying branch to Github Pages "
  (Dir["#{deploy_dir}/*"]).each { |f| rm_rf(f) }
  puts "\n## Copying #{public_dir} to #{deploy_dir}"
  cp_r "#{public_dir}/.", deploy_dir
  cd "#{deploy_dir}" do
    Rake::Task[:gitignore].execute unless File.exist?("#{deploy_dir}/.gitignore")
    Rake::Task[:git_init].execute unless File.exist?("#{deploy_dir}/.git")
    system "git add -A"
    puts "\n## Committing: Site updated at #{Time.now.utc}"
    message = "Site updated at #{Time.now.utc}"
    system "git commit -m \"#{message}\""
    puts "\n## Pushing generated #{deploy_dir} website"
    system "git push --force origin #{deploy_branch}"
    puts "\n## Github Pages deploy complete"
  end
end

task :git_init do
  puts "\n## Initializing the deployment repository"
  system "git init"  
  system "git remote add origin #{repo_url}"
end

task :gitignore do
  puts "\n## Writing a .gitignore file"
  open('.gitignore', 'w') do |post|
    post.puts "Rakefile"
  end
end

def get_stdin(message)
  print message
  STDIN.gets.chomp
end

def ask(message, valid_options)
  if valid_options
    answer = get_stdin("#{message} #{valid_options.to_s.gsub(/"/, '').gsub(/, /,'/')} ") while !valid_options.include?(answer)
  else
    answer = get_stdin(message)
  end
  answer
end

desc "list tasks"
task :list do
  puts "Tasks: #{(Rake::Task.tasks - [Rake::Task[:list]]).join(', ')}"
  puts "(type rake -T for more detail)\n\n"
end
