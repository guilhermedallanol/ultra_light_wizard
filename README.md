Ultra Light Wizard v0.1.1 (beta)
================================

No time to manage a wizard state machine, session variables, or complicated controllers? Use Ultra Light Wizard!! A RESTful session-less validation-friendly simple wizard approach in Rails.

![Ultra Light Wizard Image](https://cdn.rawgit.com/AndyObtiva/ultra_light_wizard/master/ultra_light_wizard.jpg)

This RailsConf 2014 talk video explains it all:
https://www.youtube.com/watch?v=muyfoiKHMMA

Principles
==========

- REST: wizard steps are represented as REST nested resources under the model resource being built
- MVC: respects MVC separation of concerns
- OO: honors OO principles of low coupling and high cohesion
- Design Patterns: wizard is simply a model Builder
- DDD: supports domain concepts directly with customizable vocabulary
- Non-Functional Requirements:
  - Productivity: minimum effort for adding wizards and wizard steps
  - Maintainability: minimum code to maintain while adhering to other principles
  - Performance: stateless design means scalability
  - Security: stateless design is compatible with Rails security measures

Instructions
============

Add the following to your Rails project ```Gemfile```:

```
gem 'ultra_light_wizard', '~> 0.1.1'
```

Run the command:

```
bundle
```

Then, use the following command in place of the Rails scaffold generator, and it will scaffold both standard resource components and wizard-related components:

```
rails generate ultra_light_wizard:scaffold (resource) steps:(step1),(step2),(step3),... attributes:(attribute1:db_type1),(attribute2:db_type2),...
```

This will generate wizard step routes, controller, models, and views

Example:

```
rails generate ultra_light_wizard:scaffold Project steps:basic_info,project_detail,file_uploads,preview attributes:name:string,description:text,start_date:date,delivery_date:date
```

Output:

```
generate  scaffold
  invoke  active_record
  create    db/migrate/20160201025849_create_projects.rb
  create    app/models/project.rb
  invoke    test_unit
  create      test/models/project_test.rb
  create      test/fixtures/projects.yml
  invoke  resource_route
   route    resources :projects
  invoke  scaffold_controller
  create    app/controllers/projects_controller.rb
  invoke    erb
  create      app/views/projects
  create      app/views/projects/index.html.erb
  create      app/views/projects/edit.html.erb
  create      app/views/projects/show.html.erb
  create      app/views/projects/new.html.erb
  create      app/views/projects/_form.html.erb
  invoke    test_unit
  create      test/controllers/projects_controller_test.rb
  invoke    helper
  create      app/helpers/projects_helper.rb
  invoke      test_unit
  invoke    jbuilder
  create      app/views/projects/index.json.jbuilder
  create      app/views/projects/show.json.jbuilder
  invoke  assets
  invoke    coffee
  create      app/assets/javascripts/projects.coffee
  invoke    scss
  create      app/assets/stylesheets/projects.scss
  invoke  scss
identical    app/assets/stylesheets/scaffolds.scss
    gsub  app/controllers/projects_controller.rb
    gsub  app/controllers/projects_controller.rb
  insert  app/controllers/projects_controller.rb
    gsub  app/views/projects/index.html.erb
  create  app/controllers/project_steps_controller.rb
  create  app/helpers/project_steps_helper.rb
  create  app/views/project_steps/_step_navigation.html.erb
  create  app/models/project/name.rb
  create  app/views/project_steps/name.html.erb
  create  app/models/project/description.rb
  create  app/views/project_steps/description.html.erb
  create  app/models/project/start_date.rb
  create  app/views/project_steps/start_date.html.erb
  create  app/models/project/delivery_date.rb
  create  app/views/project_steps/delivery_date.html.erb
  create  app/models/project/preview.rb
  create  app/views/project_steps/preview.html.erb
   route  resources :projects, only: [:create, :show] do
resources :project_steps, only: [:edit, :update]
end
```

Once the files are generated, you can proceed to place your own code customizations in the wizard step models and views.

To kick-off wizard, simply trigger the main model controller create action, and it will transition to the wizard step first step edit action.

For example, the following will kick-off the project wizard by creating a project and automatically redirecting to first step:

```
<%= link_to 'New Project', projects_path, method: :post %>
```

If you'd like to customize the term "step", you can add a step_alias:(alias) option as in the following:

Example:

```
rails generate ultra_light_wizard:scaffold Project step_alias:part steps:basic_info,project_detail,file_uploads,preview attributes:name:string,description:text,start_date:date,delivery_date:date
```

Output:

```
generate  scaffold
  invoke  active_record
  create    db/migrate/20160201025849_create_projects.rb
  create    app/models/project.rb
  invoke    test_unit
  create      test/models/project_test.rb
  create      test/fixtures/projects.yml
  invoke  resource_route
   route    resources :projects
  invoke  scaffold_controller
  create    app/controllers/projects_controller.rb
  invoke    erb
  create      app/views/projects
  create      app/views/projects/index.html.erb
  create      app/views/projects/edit.html.erb
  create      app/views/projects/show.html.erb
  create      app/views/projects/new.html.erb
  create      app/views/projects/_form.html.erb
  invoke    test_unit
  create      test/controllers/projects_controller_test.rb
  invoke    helper
  create      app/helpers/projects_helper.rb
  invoke      test_unit
  invoke    jbuilder
  create      app/views/projects/index.json.jbuilder
  create      app/views/projects/show.json.jbuilder
  invoke  assets
  invoke    coffee
  create      app/assets/javascripts/projects.coffee
  invoke    scss
  create      app/assets/stylesheets/projects.scss
  invoke  scss
identical    app/assets/stylesheets/scaffolds.scss
    gsub  app/controllers/projects_controller.rb
    gsub  app/controllers/projects_controller.rb
  insert  app/controllers/projects_controller.rb
    gsub  app/views/projects/index.html.erb
  create  app/controllers/project_parts_controller.rb
  create  app/helpers/project_parts_helper.rb
  create  app/views/project_parts/_part_navigation.html.erb
  create  app/models/project/name.rb
  create  app/views/project_parts/name.html.erb
  create  app/models/project/description.rb
  create  app/views/project_parts/description.html.erb
  create  app/models/project/start_date.rb
  create  app/views/project_parts/start_date.html.erb
  create  app/models/project/delivery_date.rb
  create  app/views/project_parts/delivery_date.html.erb
  create  app/models/project/preview.rb
  create  app/views/project_parts/preview.html.erb
   route  resources :projects, only: [:create, :show] do
resources :project_parts, only: [:edit, :update]
end
```

To learn more about the Ultra Light Wizard philosophy and function, please read this [Code Painter](http://www.codepainter.ca) blog post: [Ultra Light & Maintainable Wizard in Rails] (http://www.codepainter.ca/2013/10/ultra-light-maintainable-wizards-in.html)

Features
========

- Ultra Light Wizard scaffold generator
  + [DONE] Scaffolding of main model controller/views/migration
  + [DONE] Routes
  + [DONE] Controller steps
  + [DONE] Model parts
  + [DONE] View parts
  + [DONE] View navigation
  + [DONE] Helper for ultra light wizard support
  + [DONE] Route helper methods
  + [DONE] Wizard kick-off helper/view
  + [DONE] Forms
  + [DONE] Support for attributes
  + [DONE] Form fields
  + [DONE] Custom name conventions
  - Write automated tests
  - Support for nested resources
  - Modularize (perhaps extracting sub-generators)

Enhancements
============

If you are interested in having enhancements implemented, please report via a GitHub issue, describing the problem, providing use case examples, and suggesting solutions to implement. I'd be happy to implement as part of paid work if needed ASAP using the [CodeMentor platform](https://www.codementor.io/andymaleh).

License
=======

MIT
