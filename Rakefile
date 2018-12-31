include FileUtils
require 'rake-jekyll'
require 'tmpdir'

# This task builds the Jekyll site and deploys it to a remote Git repository.
# It's preconfigured to be used with GitHub and Travis CI.
# See http://github.com/jirutka/rake-jekyll for more options.
Rake::Jekyll::GitDeployTask.new(:deploy) do |t|
	t.committer = 'Jack Liu <jackliusr@gmail.com>'
	t.deploy_branch = 'master'

end

