Usage
from dialog_bot_sdk.bot import DialogBot
import grpc
import os


def on_msg(*params):
    for param in params:
        bot.messaging.send_message(param.peer, 'Reply to : ' + str(param.message.text_message.text))


if __name__ == '__main__':
    bot = DialogBot.get_secure_bot(
        os.environ.get('BOT_ENDPOINT'),     # bot endpoint from environment
        grpc.ssl_channel_credentials(),     # bot credentials
        os.environ.get('BOT_TOKEN'),        # bot token from environment
        **os.environ.get('BOT_OPTIONS') if os.environ.get('BOT_OPTIONS') is not None else {}
        # options:
        #     verbose: verbosity level of functions calling (bool)
        #     options: channel's options (dict)
        #     retry_options: dict of retries options (delay_factor, min_delay, max_delay, max_retries) (dict)
        #     rate_limiter_options: dict of rate limiter options (max_calls, period) (dict)
        #     root_certificates: path to PEM-encoded certificates
    )

    bot.messaging.on_message(on_msg, is_async=True)
