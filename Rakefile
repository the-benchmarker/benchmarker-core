# frozen_string_literal: true

require("bundler/gem_tasks")
task(default: :spec)

require("active_record")
require("active_record/database_configurations/database_config")
require("active_record/database_configurations/url_config")

database_url = ENV.fetch("DATABASE_URL")

namespace :db do
  task :create do
    database = ActiveRecord::DatabaseConfigurations::UrlConfig.new(nil, nil, database_url)

    ActiveRecord::Tasks::DatabaseTasks.create(database.config)
  end

  task :migrate do
    base_path = Gem.loaded_specs["benchmarker-data"].full_gem_path
    full_path = File.join(base_path, "db", "migrations")
    ActiveRecord::MigrationContext.new(full_path, ActiveRecord::SchemaMigration).migrate
  end

  task :drop do
    database = ActiveRecord::DatabaseConfigurations::UrlConfig.new(nil, nil, database_url)

    ActiveRecord::Tasks::DatabaseTasks.drop(database.config)
  end

  task :seed do
    require("benchmarker/data")

    include Benchmarker::Data

    10.times do |i|
      Language.create(label: "language_#{i + 1}")
    end

    10.times do |i|
      Framework.create(label: "framework_#{i + 1}", language_id: i + 1)
    end

    10.times do |i|
      Concurrency.create(level: i + 1)
    end

    10.times do |i|
      Key.create(label: "key_#{i + 1}")
    end

    10.times do |i|
      Metric.create(framework_id: i + 1, value_id: i + 1, concurrency_id: i + 1)
    end

    10.times do |i|
      Value.create(value: i + 1, key_id: i + 1)
    end
  end
end
