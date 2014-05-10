Putting Your Application Online with OpenShift
==============================================

What is deployment?
-----------------
Your app is live. Great! But right now, it runs on your local machine only. If you want others to see and use it, you'll need to deploy it. A fast way to deploy your app is to push it to OpenShift using Git.

Deploying to OpenShift 
-----------------
1. If you didn't [create an OpenShift Online account](https://www.openshift.com/app/account/new) or [install the RHC client tools](https://www.openshift.com/developers/rhc-client-tools-install) as part of the ClojureBridge setup process, do this now (follow the links for instructions).
2. On the command line, execute the comand `rhc setup` to configure RHC to communicate with OpenShift.  
3. Make sure you're in the global-growth directory. (<code>cd global-growth</code> if necessary.) Now run <code>git init</code> to create an empty Git repository.
4. Run the following command to create an auto-scaling OpenShift Clojure app called _globalgrowth_: <pre><code>rhc app create globalgrowth http://cartreflect-claytondev.rhcloud.com/github/openshift-cartridges/clojure-cartridge -s --no-git</code></pre>
In the output, find a line that looks like this and copy the SSH URL: <pre><code>Git remote: ssh://0123456789abcdef01234567@globalgrowth-yourdomain.rhcloud.com/~/git/globalgrowth.git/</code></pre>
5. Run the following command to tell your local Git repository where to find your OpenShift Git repository, replacing the SSH URL with the one you copied: <pre><code>git remote add openshift ssh://0123456789abcdef01234567@clojureapp-yourdomain.rhcloud.com/~/git/clojureapp.git/</code></pre>
6. There are three small changes to make to Global Growth to configure it to run on OpenShift. Firstly, open _project.clj_ and change the value for _:main_ from _global-growth.core_ to _global-growth.web_. This makes _web.clj_ the place Clojure looks for a main function when you issue the command _lein run_.
7. Next, open _src/global_growth/web.clj_. At the top of the file, add a require statement for Jetty like this: <code>[ring.adapter.jetty :as jetty]</code>. Your require block should now look something like this: <pre><code>(ns global-growth.web
  (:require [global-growth.core :as api]
            [compojure.core :refer [defroutes GET]]
            [compojure.handler :refer [site]]
            [hiccup.core :as hiccup]
            [hiccup.page :as page]
            [hiccup.form :as form],
            [ring.adapter.jetty :as jetty]))
</code></pre>
8. Finally, at the very bottom of _web.clj_, add the following code: <pre><code>(defn -main
      [& args]
      \(jetty/run-jetty handler {:port (Integer/parseInt (get (System/getenv) "OPENSHIFT_CLOJURE_HTTP_PORT" "3000"))
                                :host (get (System/getenv) "OPENSHIFT_CLOJURE_HTTP_IP" "0.0.0.0")}))
</code></pre>
This tells the app what port and host to use when it is running on OpenShift, using environment variables. The app will use the default values shown (3000/0.0.0.0) when the OPENSHIFT environment variables are not present, such as when you issue the command `lein run` locally.
9. OpenShift will receive only the files you have committed to your local repo, so it's time to add your files to Git and commit and push these changes to the repo you've just created.
Run <code>git status</code>. This command shows you the files you've modified but have yet to stage for a commit. Run the following commands to stage all your files, commit the corresponding changes, and push the code to OpenShift:
	<pre><code>git add .
git commit -m "[Your commit message here]"
git push -f openshift master</code></pre>
Your commit message should refer to the changes you've made. Something like "Add functions to retrieve indicator data" would work. 
The `-f` for force is only needed the first time you push the code, to tell Git to completely override the template content that is in the OpenShift repository by default. If you make more changes to the app code, you can deploy them on OpenShift with another cycle of `git add .`, `git commit -m "[Your commit message here]"`, and `git push openshift master`.
10. Paste the app URL in a browser to see the app live on the web. You can check the URL with the following command: <pre><code>rhc app show -a globalgrowth</code></pre>
