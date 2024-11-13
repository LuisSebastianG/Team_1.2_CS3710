# Student Portfolio: Add Projects Design and Implementation

[Technical Documentation](#technical-documentation)

[Development](#development)

[Setting Up for Development](#setting-up-for-development)

[Creating Data Structure](#creating-data-structure)

[Nested Routes](#nested-routes)

[Models](#models)

[Project Models](#project-models)

[Student Model](#student-model)

[Controller](#controller)

[Views](#views)

[Student](#student)

[Projects](#projects)

# Technical Documentation {#technical-documentation}

What technical documentation are you responsible for M06? 
I am responsible for the documentation on one to many relationships

# Development {#development}

Keep track of what you do. Include notes on what you customized, file name, code you added or changed 

## **Setting Up for Development** {#setting-up-for-development}

List what steps you did before starting development

| Before even starting development we have to decide how to prepare the portfolio_app for this specific development. In this case we would start with truncating the database to make sure there is no "bad" data.|
| :---- |

## **Creating Data Structure** {#creating-data-structure}

Scaffold notes:

| Command: |
| :---- |
```bash
rails generate scaffold Project title:string description:text student_id:references
```

| Schema for Project: Project creates a table for the projects to be stored in the database. title:string is creating a string named title. description:text is making a text with the name description in the database. Finally, student_id:references creates another object named student_id which will hold the foreign_key |
| :---- |

## **Nested Routes** {#nested-routes}

Routes notes:

| File: config/routes.rb code: |
| :---- |
```ruby
  # Necessary Student routes and  portfolio routes
  resources :students, only: [:index, :show, :edit, :update, :destroy] do
    resource :portfolio, only: [:show, :edit, :update]  # Nesting portfolio under student
    resources :projects # Nesting projects under students
  end
```
## **Models** {#models}

### **Project Models**  {#project-models}

Notes:

| File: app/models/project.rb Only put code pertaining to new project model Code:  |
| :---- |

### **Student Model**  {#student-model}

Notes:

| File: app/models/student.rb Code:    |
| :---- |

## **Controller** {#controller}

File:  
Customization Notes:

| File: app/controllers/projects\_controller.rb Code: |
| :---- |
```ruby
#creates a local @student instance variable by finding a student instance by student_id
  def get_student
    @student = Student.find(params[:student_id])
  end
```
```ruby
class ProjectsController < ApplicationController
  #ensures that get_student runs before each action defined in the file
  before_action :get_student
```
```ruby
  def index
    #return all project instances associated with a particular student instance
    @projects = @student.projects
  end
```
```ruby
  def new
    #creates a project object that's associated with the specific student instance from get_student
    @project = @student.projects.build
  end
```
```ruby
  def create
    @project = @student.projects.build(project_params)

    respond_to do |format|
      if @project.save
        format.html { redirect_to student_projects_path(@student), notice: "Project was successfully created." }
        format.json { render :show, status: :created, location: @project }
      else
        format.html { render :new, status: :unprocessable_entity }
        format.json { render json: @project.errors, status: :unprocessable_entity }
      end
    end
  end
```
```ruby
  def update
    respond_to do |format|
      if @project.update(project_params)
        #redirect to student_project_path(@student), which will redirect to the index view of the selected student's projects
        format.html { redirect_to student_project_path(@student), notice: "Project was successfully updated." }
        format.json { render :show, status: :ok, location: @project }
      else
        format.html { render :edit, status: :unprocessable_entity }
        format.json { render json: @project.errors, status: :unprocessable_entity }
      end
    end
  end
```
```ruby
  def destroy
    @project.destroy!

    respond_to do |format|
      format.html { redirect_to student_projects_path(@student), notice: "Project was successfully destroyed." }
      format.json { head :no_content }
    end
  end
```

## **Views** {#views}

Look at the [http://localhost:3000/rails/info/routes](http://localhost:3000/rails/info/routes) to help update links in views

### **Student**  {#student}

Views  Notes:

| File: app/views/students/\show.html.erb Code: |
| :---- |
```ruby
<h2>Projects</h2>
<% for project in @student.projects %>
    <ul>
    <li><%= project.body %>
  </ul>
<% end %>
```
```ruby
  <%= link_to "Edit this student", edit_student_path(@student) %> 
  <%= link_to 'Add Project', student_projects_path(@student) %>
  <%= link_to "Back to students", students_path %>
```

### **Projects**  {#projects}

Views  Notes:

| File: app/views/projects/edit.html.erb Code: |
| :---- |
```ruby
  <%= link_to "Show this project", [@student, @project] %> |
  <%= link_to "Back to projects", student_projects_path(@student) %>
```

Views  Notes:

| File: app/views/projects/_form.html.erb Code: |
| :---- |
```ruby
<%= form_with(model: [@shark, post], local: true) do |form| %>
```

Views  Notes:

| File: app/views/projects/new.html.erb Code: |
| :---- |
```ruby
  <%= link_to "Back to projects", student_projects_path(@student) %>
```

Views  Notes:

| File: app/views/projects/index.html.erb Code: |
| :---- |
```ruby
<table>
  <thead>
    <tr>
      <th>Body</th>
      <th>Student</th>
      <th colspan="3"></th>
    </tr>
  </thead>

  <tbody>
  <% @projects.each do |project| %>
    <tr>
      <td><%= project.body %></td>
      <td><%= project.student.name %></td>
      <td><%= link_to 'Show Student', [@student] %></td>
      <td><%= link_to 'Edit Project', edit_student_project_path(@student, project) %></td>
      <td><%= link_to 'Destroy Project', [@student, project], method: :delete, data: {confirm: 'Are you sure?'} %></td>
    </tr>
  <% end %>
  </tbody>
</table>
```
