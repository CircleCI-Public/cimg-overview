# Philosophy

How do we handle package updates and security/CVE?

For Convenience Images, once we publish a specific release, we don't republish it unless there's a severe bug or security problem. For the majority of these CVEs you use, the aren't a security concern in our environment due to the nature of how CircleCI and Docker works. So on paper they look like an issue but typically they're not.
In the rare case where a CVE would affect customers, we then republish that image.
If there are specific packages that the customer wants updated still, the fastest way to do this typically is for them to update them in their build. Something like sudo apt-get update && sudo apt-get install package1 package2. This will update only the packages they care about to the latest patch versions that Ubuntu itself carries.
Other options include rolling their own custom image, extending ours, using the Docker Library Node.js image, etc, those those are varying degrees of additional work.
