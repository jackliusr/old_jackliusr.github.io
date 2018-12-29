require 'rake-jekyll'

# This task builds the Jekyll site and deploys it to a remote Git repository.
# It's preconfigured to be used with GitHub and Travis CI.
# See http://github.com/jirutka/rake-jekyll for more options.
Rake::Jekyll::GitDeployTask.new(:deploy) do |t|
  t.committer = 'Jekyll Publisher <jekyll@example.com>'
end


namespace :deploy do
  desc "push built site to target branch"
  task :s3 do
    TARGET_BRANCH = 's3'
    SLUG = ENV['TRAVIS_REPO_SLUG']
    USER = SLUG.split("/")[0]
    TOKEN = ENV['ACCESS_TOKEN']
    COMMIT_MSG = "Site updated via #{ENV['TRAVIS_COMMIT']}".freeze
    ORIGIN = "https://#{USER}:#{TOKEN}@github.com/#{SLUG}.git".freeze
    puts "Deploying to #{TARGET_BRANCH} branch from Travis as #{USER}"

    Dir.mktmpdir do |tmp|
      cp_r '_site/.', tmp
      Dir.chdir tmp
      system 'git init'
      system "git add . && git commit -m '#{COMMIT_MSG}'"
      system 'git remote add origin ' + ORIGIN
      system "git push origin master:refs/heads/#{TARGET_BRANCH} --force"
    end
  end

  desc "push built site to target branch"
  task :master do
    TARGET_BRANCH = 'master'
    SLUG = ENV['TRAVIS_REPO_SLUG']
    USER = SLUG.split("/")[0]
    REPO = SLUG.split("/")[1]
    TOKEN = ENV['ACCESS_TOKEN']
    COMMIT_MSG = "Site updated via #{ENV['TRAVIS_COMMIT']}".freeze
    ORIGIN = "https://#{USER}:#{TOKEN}@github.com/#{SLUG}.git".freeze
    puts "Deploying to #{TARGET_BRANCH} branch from Travis as #{USER}"

    rm_rf('_site')

    opts = {
      'source' => '.',
      'destination' => '_site',
      'config' => '_config.yml',
      'baseurl' => "/#{REPO}"
    }

    Jekyll::Site.new(Jekyll.configuration(opts)).process
    Dir.mktmpdir do |tmp|
      cp_r '_site/.', tmp
      Dir.chdir tmp
      system 'git init'
      system "git add . && git commit -m '#{COMMIT_MSG}'"
      system "git remote add origin #{ORIGIN}"
      system 'git push origin master:refs/heads/master --force'
    end
  end
end
