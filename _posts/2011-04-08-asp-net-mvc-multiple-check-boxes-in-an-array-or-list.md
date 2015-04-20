---
layout: post
title: Asp.net MVC Multiple check-boxes in an array or list
date: 2011-04-08 19:27
author: shawnmclean
comments: true
categories: [.Net Framework, asp.net-mvc, asp.net-mvc-3]
---
I had the problem of having a list of check-boxes being displayed in my view.

I'll be using a scenario many of us are familiar with, <em>Users </em>and <em>Roles. </em>I'll be using ASP.NET MCV 3 with the Razor view engine syntax.

Here is my ViewModel:

[codesyntax lang="csharp"]
<pre>    public class UserRoleViewModel
    {
        public UserRoleViewModel()
        {
            Roles = new List&lt;RoleViewModel&gt;();
        }
        public string UserId { get; set; }
        public List&lt;RoleViewModel&gt; Roles { get; set; }
    }

    public class RoleViewModel
    {
        public bool IsInRole { get; set; }
        [HiddenInput(DisplayValue = false)]
        public int RoleId { get; set; }
        [HiddenInput(DisplayValue = true)]
        public string RoleName { get; set; }
    }</pre>
[/codesyntax]

Now we need our <em>Actions</em>:

[codesyntax lang="csharp"]
<pre>        public ActionResult EditUserRole(string userId)
        {
            var user = userService.GetUser(userId);
            var roles = userService.GetRoles();

            UserRoleViewModel viewModel = new UserRoleViewModel();
            viewModel.UserId = user.UserId;
            //Match the roles the user is in with the list of roles
            foreach (var role in roles)
            {
                viewModel.Roles.Add(new RoleViewModel
                                        {
                                            IsInRole = user.Roles.Any(r =&gt; r.RoleId == role.RoleId),
                                            RoleId = role.RoleId,
                                            RoleName = role.RoleName
                                        });
            }

            return View(viewModel);
        }

        [HttpPost]
        public ActionResult EditUserRole(UserRoleViewModel model)
        {
            List&lt;Role&gt; roles = model.Roles.Where(r =&gt; r.IsInRole).Select(r =&gt; new Role {RoleId = r.RoleId, RoleName = r.RoleName}).ToList();
            userService.AddRolesToUser(model.UserId, roles);

            return View();            
        }</pre>
[/codesyntax]

Now we need to setup our view. This is our <strong>EditUserRole.cshtml</strong>

[codesyntax lang="csharp"]
<pre>@model WebUI.ViewModel.UserRoleViewModel

@using (Html.BeginForm("EditUserRole", "Administrator"))
{
    @Html.HiddenFor(x =&gt; Model.UserId)

    @Html.EditorFor(x =&gt; Model.Roles)

    &lt;input type="submit" /&gt;
}</pre>
[/codesyntax]

This is our editor template, located at: <strong>~/Shared/EditorTemplates/RoleViewModel.cshtml </strong>. Make sure the name of the file is the same name as the viewmodel you are sending.

[codesyntax lang="csharp"]
<pre>@model WebUI.ViewModel.RoleViewModel

@Html.CheckBoxFor(m =&gt; m.IsInRole)
@Html.HiddenFor(m =&gt; m.RoleId)
@Html.LabelFor(m =&gt; m.IsInRole, Model.RoleName)
&lt;br /&gt;</pre>
[/codesyntax]

And thats it. No explanation needed.
