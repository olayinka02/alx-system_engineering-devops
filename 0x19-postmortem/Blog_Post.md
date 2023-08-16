Postmortem



Issue Summary
Just after unveiling a new feature on our recently launched Ruby on Rails site, we encountered a wave of user complaints. Merely five minutes after implementing the update, a flurry of emails poured in from users reporting an inability to sign in or sign up on our platform. This caught us off guard, as the feature had been functioning flawlessly on our own machines and in prior tests. The inbox was flooded with around 127 such emails – an overwhelming surge. Realizing the critical importance of user acquisition and retention, the prospect of losing 127 users due to this issue was unacceptable. Determined to address the problem, we took immediate action. Retrieving the site's repository from GitBug, we meticulously followed the installation instructions as laid out in the README. To our astonishment, the site failed to launch. The root of the problem soon became evident – our oversight in updating the project's requirements. This glitch left the site nonfunctional from 9:55 AM GMT+1 to 11:20 AM GMT+1.



Timeline
9:55 AM GMT+1: A customer reported an issue regarding their inability to sign in to the site.
10:20 AM GMT+1: Winus, a backend developer, encountered the same sign-in problem that customers had reported.
10:35 AM GMT+1: Our team initiated an investigation into potential inconsistencies within the controllers and views of the site.
10:40 AM GMT+1: Suspecting the bcrypt gem, one of the site's dependencies, to be the source of the issue, we speculated that it might either be faulty or improperly utilized. The error message on the site indicated an error raised by the bcrypt gem due to an invalid hash.
10:42 AM GMT+1: We examined whether the views were correctly binding the form fields to the appropriate model fields, but this hypothesis later proved to be incorrect.
10:45 AM GMT+1: Our assumption that the controllers might be generating a different hash for a valid admin password turned out to be misleading.
10:50 AM GMT+1: Winus proposed that the problem could stem from an improper password hashing process.
11:00 AM GMT+1: The backend development team escalated the incident for more focused attention.
11:20 AM GMT+1: The incident was successfully resolved by updating the backend server's requirements, specifically the version of the bcrypt gem used.


Root Cause And Resolution
Regarding the bcrypt gem, the version we were utilizing had become outdated. Despite the hash being valid and matching the database records, the gem was generating an error. This could possibly stem from the hash format not being compatible with the installed bcrypt version. Winus expertly resolved the situation by manually updating the Gemfile.lock's bcrypt version to a more current one, followed by a reinstallation of the required gems. The solution proved to be highly effective.

Actions for Resolution and Prevention:
Implement a continuous integration pipeline that automatically conducts builds on each pull request branch. This guarantees that the pull request branch must pass the builds before merging into the deployment branch.
Establish a robust monitoring system for both the database and application servers. This proactive approach will help identify and address any arising issues promptly.
Develop comprehensive tests that must successfully pass for every new feature. These tests should be obligatory for merging with the deployment branch, thus ensuring the stability of the system.
