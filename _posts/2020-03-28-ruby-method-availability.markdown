---
layout: post
title: "Ruby Method Availability"
date: 2020-03-28 00:00:00-0600
categories: jekyll update
---
I learned a great ruby tip this week for getting a specific list of methods available on an object, so I thought I'd make a post about it. The first part of the post explains the tip, and then the second part goes into how I recently used it.

### Question
Is there a way to quickly see a list of all of the methods available for a ruby object that does not include the methods that object inherits from the `Object` class?

### Answer
Yes. `{your_object. methods} - Object.methods`

### Details
I have been using the [methods](https://ruby-doc.org/core-2.7.0/Object.html#method-i-methods) method for a long time and have always found it a bit annoying that the result of calling `{your_object.methods}` always includes the methods on the `Object` class. It makes perfect sense why this is the case, but typically when I am in search of available methods I only care about seeing the methods that are specific to the object I am troubleshooting. So, to get around this we can leverage ruby’s [-](https://ruby-doc.org/core-2.7.0/Array.html#method-i-2D) method to and get a list that is not polluted with the methods we don’t care about.

### Bug I Was Working On
While the list of situations where this tip could come in handy is endless, I wanted to quickly explain how I used it this week so people can see an example.

At GoNoodle we’ve recently been betting a lot of errors while uploading users to [CloudSearch](https://aws.amazon.com/cloudsearch/). It’s been a bit tricky to debug because it happens intermittently with no clearly discernible pattern (the best type of bug), so instead of endlessly spinning my wheels on the issue I decided to leverage our [BugSnag](https://www.bugsnag.com/) integration. The method looked like this before I added my debug code.

```
  def upload_documents(documents)
    @client.upload_documents(documents: documents.to_json, content_type: 'application/json')
  end
```
and afterwards it looked like this.
```
  def upload_documents(documents)
    begin
      @client.upload_documents(documents: documents.to_json, content_type: 'application/json')
    rescue StandardError => e
      Bugsnag.notify("Cannot be added to CloudSearch. documents:#{documents}, message: #{e.message}, code: #{e.code}")
    end
  end
```
What this new code does is ensure that we get a BugSnag notification (with details) every time an `Aws::CloudSearchDomain::Errors::DocumentServiceException` exception is raised (this is what happens when a user is not uploaded to CloudSearch).

### How I Used The Tip
When I was implementing the code above I could not remember the [Exception ](https://ruby-doc.org/core-2.5.1/Exception.html) methods that were available for providing details on the exception (`code` and `message`), so I quickly put a `binding.pry` (see [pry](https://github.com/pry/pry)) in the rescue block and ran `e.methods - Object.methods` and got the following list: `[:code, :message, :full_message, :backtrace_locations, :backtrace, :cause, :exception, :set_backtrace, :blamed_files, :describe_blame, :copy_blame!, :blame_file!]`. This gave me exactly what I needed!

### Conclusion
In summary, the next time you find yourself wondering what methods are available on an object and you don't care about seeing the methods from the `Object` class, run `{your_object. methods} - Object.methods` and you should be good to go!
