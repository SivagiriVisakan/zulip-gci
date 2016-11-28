# GCI Tasks: Intro to Zulip server development

## Prerequisites

* A working Zulip development environment. See
  https://github.com/zulip/zulip-gci/blob/master/README.md for instructions
  on how to set one up.

## Practice Running Zulip

Run your Zulip server using
```
tools/run-dev.py
```

Visit your Zulip server at `http://<hostname>:9991`, where `<hostname>` is
either `localhost` or the hostname of your remote VM, and verify that you
can log in by clicking on one of the accounts.

## Make a Change.

Find the `check_send_message` function by going to the `zulip/` directory
and entering `git grep 'def check_send_message'`.

Modify `check_send_message` so that anytime a user sends a message which says
"Nanananana" the message becomes "Nanananana Batman!"

Test that your change works in the browser, and take a screenshot.

## Add Some Tests

Add some tests to the bottom of `zerver/tests/test_messages.py` to make sure
we do not accidentally break this behavior in the future.

```
class BatmanTest(ZulipTestCase):
    def test_add_batman_to_nanana_message(self):
        # type: () -> None
        sender = get_user_profile_by_email('othello@zulip.com')
        client = make_client(name="test suite")
        message_id = check_send_message(sender, client, "stream", ["Verona"], "Batman test", "Nanananana")
        self.assertEqual(Message.objects.values_list("content", flat=True).get(id=message_id),
                         "Nanananana Batman!")

    def test_do_not_add_batman_to_normal_message(self):
        # type: () -> None
        sender = get_user_profile_by_email('othello@zulip.com')
        client = make_client(name="test suite")
        message_id = check_send_message(sender, client, "stream", ["Verona"], "Batman test", "Hi there")
        self.assertEqual(Message.objects.values_list("content", flat=True).get(id=message_id),
                         "Hi there")
```

Then run
```
tools/test-backend zerver/tests/test_messages.BatmanTest
```
to make sure your code passes the tests you just added. If it doesn't,
fix any brokenness in your code until it does. Take a screenshot of
your terminal.

Run
```
./tools/test-all
```
to verify that your code still passes the entire test suite.

`tools/test-all` is a suite of several tests, and can take a while to run. For
interactive debugging, one often wants to just run a single test in the suite. Run
```
cat tools/test-all
```
and figure out how to run just the lint tests and do so (there should be no
output). Add an extra space to the beginning of any line of code and run the
lint tests again. You should see a bunch of output in red. Take a screenshot of
your terminal.

## Submit

Submit the three screenshots using the GCI tasks interface.