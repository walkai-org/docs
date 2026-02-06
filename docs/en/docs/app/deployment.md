# Deployment

After configuring all repository variables and secrets, run the GitHub Actions workflow `Apply Infra (Bootstrap)`.
To run a workflow, go to the **Actions** tab, select it, click **Run workflow**, and confirm the run.

Once it finishes, open the job named `show_name_servers` and copy the four name server values it prints. Before continuing, you must add those name servers in your domain manager (if you already use one) under the `walkai` subdomain of your `BASE_DOMAIN`. If you manage the domain at the registrar, add them there instead.

When the name servers are in place, run the `Apply Infra (Finalize HTTPS)` workflow the same way as before.

Finally, check the inbox for `BOOTSTRAP_FIRST_USER` and follow the instructions in that email to complete the setup.

**Optional:** if you need to tear down the infrastructure, you can run the `Destroy Infra` workflow.
