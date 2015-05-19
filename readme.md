# Membership Utility Installation Notes

## The Membership Utility has several core functions:
Query a User to see which Permission Set(s), Public Group(s), and Queue(s) he/she belongs to query a Permission Set, Public Group, or Queue to see a list of its members. It leverages 3 hidden multi-picklists on the User record that track Permission Set(s), Public Group(s), and Queue(s) and a batch that runs nightly to update these fields with any changes in membership.

### It is manifested in several ways:
- A Visualforce page to query a User to see which Permission Set(s), Public Group(s), and Queue(s) he/she belongs to; 
- 3 Visualforce pages, one each to query a Permission Set, Public Group, and Queue to see a list of their respective members; 
- A component on a Salesforce System Case to automatically query the Profile, Permission Sets, Public Groups, and Queues of the Case Contact  reporting on the 3 hidden multi picklists

Version 2 will replace the 3 multi picklists with a Custom Object and Related List that tracks "SObjects", including Permission Set(s), Public Group(s), and Queue(s). This will actually require a Junction Object since the relationships are many-to-many. It will be linked to the SObjects in Application Inventory.

## Folder Contents:
- readme.md – describes core functions
- src – source folder created using eclipse on checkout from a dev org; may be used to deploy to another org.

Alternative to using eclipse or Ant—simply copy+paste apex code into salesforce using the SF text editor.

Required fields on User object:
![Required fields on User Object](/img/reqd_fields_user_object.png)

Required Link on Case object:
![Required Link on Case Object](/img/reqd_link_case_object.png) 

To create Links:
Go to Setup > Build > Customize > Cases > Buttons,Links, and Actions 
![Buttons, Links, and Actions](/img/button_link_action.png)
	 

 
1. Permission Set Assignees ![Permission Set Assignees](/img/permission_set_assignees.png)
1. Public Group Assignees ![Public Group Assignees](/img/public_group_assignees.png)
1. Queue Assignees ![Queue Assignees](/img/queue_assignees.png)
1. User Memberships ![User Memberships](/img/user_membership.png)

Add above Links to the Case Page Layout Custom Links by dragging …
![Case Page Layout](/img/case_page_layout_custom_links.png)

## VF Page on Case Page Layout

Go to Cases … Page Layouts, and select/edit the desired Page Layout.
![Visual Force Page Layout - Case](/img/vf_page_layout.png)

Create a New Section by dragging the Section icon/button to the page and label it 'Profile and Permission Sets'. 
![New Section Profiled and Permission Sets](/img/profile_permissions_section.png)

Then Drag the VF “caseContactPS” to the newly created section. 
![caseContactPS](/img/vf_drag_section.png)

![caseContactPS Properties](/img/vf_properties.png)


To Schedule nightly job: fasUserRelatedGroupingsScheduler, run on DeveloperConsole the following script:

    fasUserRelatedGroupingsScheduler sh1 = new fasUserRelatedGroupingsScheduler();
    String sch = '0 45 22 ? * MON-FRI';   // 10:00PM Monday thru Friday
    String JobId = system.schedule('CEO User Related Groupings Batch Update', sch, sh1);

![Apex Code Example](/img/apex_code_example.png)

Sample Output:
![Sample Output](/img/sample_output.png)
