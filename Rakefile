desc "Build _site/ for production"
task :build do
  puts "\n## Building Jekyll to _site/"
  status = system("JEKYLL_ENV=production bundle exec jekyll build")
  puts status ? "Success" : "Failed"
end

desc "Commit _site/"
task :commit, [:commit_name] do |t, args|
  puts "\n## Staging modified files"
  status = system("git add -A")
  puts status ? "Success" : "Failed"
  puts "\n## Committing a site build at #{Time.now.utc}"
  message = "Build site at #{Time.now.utc}. " + args[:commit_name].to_s
  status = system("git commit -m \"#{message}\"")
  puts status ? "Success" : "Failed"
  puts "\n## Pushing commits to remote"
  status = system("git push origin source")
  puts status ? "Success" : "Failed"
end

desc "Deploy _site/ to main branch"
task :deploy do
  puts "\n## Deleting main branch"
  status = system("git branch -D main")
  puts status ? "Success" : "Failed"
  puts "\n## Creating new main branch and switching to it"
  status = system("git checkout -b main")
  puts status ? "Success" : "Failed"
  puts "\n## Forcing the _site subdirectory to be project root"
  status = system("git filter-branch --subdirectory-filter _site/ -f")
  puts status ? "Success" : "Failed"
  puts "\n## Switching back to source branch"
  status = system("git checkout source")
  puts status ? "Success" : "Failed"
  puts "\n## Pushing all branches to origin"
  status = system("git push --all origin --force")
  puts status ? "Success" : "Failed"
end

desc "Commit and deploy _site/"
task :publish, [:commit_name] => [:build, :commit, :deploy] do
end
