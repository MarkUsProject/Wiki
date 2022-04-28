# Action Mailer in MarkUs

The Rails ActionMailer is used in MarkUs to send automated emails when grouping submissions are released, grade entry forms are released, and students invite other students to groupings. These calls to the mailer are in the files `submissions_helper.rb`, `grade_entry_forms_controller.rb`, and `groups_controller.rb` respectively. Students opt in to receiving emails by default, but can choose to unsubscribe by changing their preferences through two attributes: `receives_results_emails` and `receives_invite_emails`.

## Configuring the Development Environment

When using the ActionMailer features of MarkUs, the MarkUs administrators configure an email server. For the sake of development, you can configure MarkUs to send emails from a service you already use (such as Gmail, Outlook, etc). You can do so by changing the development environment with the `config.action_mailer` values. To get you started quickly, here we will talk specifically about configuring action mailer with a gmail account using the `:smtp` delivery method.

1. Find the `config/settings/development.yml` file.
2. Ensure action mailer is configured as follows:

    ```yaml
    action_mailer:
        delivery_method: smtp
        default_url_options:
            host: 'localhost:3000'
        asset_host: 'http://localhost:3000'
        perform_deliveries: true
        deliver_later_queue_name: ~
        smtp_settings:
            address: smtp.gmail.com
            port: 587
            user_name: # Your gmail username
            password: # Your created application password. See steps 3-4 for more details
            authentication: plain
            enable_starttls_auto: true
    ```

3. Generate an app password for MarkUs. See [this](https://support.google.com/accounts/answer/185833?hl=en) page for instructions on how to do this.
4. Copy and paste the app password into the `password` field under `smtp` settings.
5. Start the MarkUs server: `docker compose up rails`.

**Remember to never commit your email and password.**

## For Troubleshooting Email Issues

If you have issues with emails not sending due to authentication issues (in particular gmail), you can reference the `Troubleshooting Email Sending Errors` section in [this guide.](https://dev.to/morinoko/sending-emails-in-rails-with-action-mailer-and-gmail-35g4)

In the case that your issue is not solved by that information, head to [the Action Mailer guide.](https://guides.rubyonrails.org/action_mailer_basics.html)
