<template>
    <div class="hello">
        <h1>{{ msg }}</h1>

        <div class="box box-success direct-chat direct-chat-success">
            <div class="box-header with-border">
                <h3 class="box-title">Direct Chat</h3>

                <div class="box-tools pull-right">
                    <span data-toggle="tooltip" title="3 New Messages" class="badge bg-green">3</span>
                    <button type="button" class="btn btn-box-tool" data-widget="collapse"><i class="fa fa-minus"></i>
                    </button>
                    <button type="button" class="btn btn-box-tool" data-widget="remove"><i class="fa fa-times"></i></button>
                </div>
            </div>
            <!-- /.box-header -->
            <div class="box-body">

                <!-- Conversations are loaded here -->
                <div class="direct-chat-messages">
                    <!-- Message. Default to the left -->
                    <div class="direct-chat-msg" v-for="notification in notifications">
                        <div class="direct-chat-info clearfix">
                            <span class="direct-chat-name pull-left">Pushwoosh</span>
                            <span class="direct-chat-timestamp pull-right">{{ notification.date }}</span>
                        </div>
                        <!-- /.direct-chat-info -->
                        <img class="direct-chat-img" src="bower_components/AdminLTE/dist/img/user1-128x128.jpg" alt="Message User Image"><!-- /.direct-chat-img -->
                        <div class="direct-chat-text">
                            {{ notification.message }}
                        </div>
                        <!-- /.direct-chat-text -->
                    </div>
                    <!-- /.direct-chat-msg -->
                </div>
                <!--/.direct-chat-messages-->


            </div>
            <!-- /.box-body -->
            <div class="box-footer">
                <div class="input-group">
                    <input id="message" type="text" name="message" placeholder="Type Message ..." class="form-control">
                  <span class="input-group-btn">
                    <button v-on:click="notify" type="button" class="btn btn-success btn-flat">Send</button>
                  </span>
                </div>
            </div>
            <!-- /.box-footer-->
        </div>

    </div>
</template>

<script>
    export default {
        data () {
            return {
                msg: 'Hello World!',
                notification_message: 'Jolo Compolo',
                notifications: notificationStorage.fetch()
            }
        },
        methods: {
            notify: function() {

                var notification = {message:$('#message').val(), date:"Now"};
                this.notifications.push(notification);

                notificationStorage.save(this.notifications);

                $.ajax({
                    type: "POST",
                    url: "https://cp.pushwoosh.com/json/1.3/createMessage",
                    data: JSON.stringify({
                        "request": {
                            "application": "4FC89B6D14A655.46488481",
                            "auth": "mTdns0j6qLYPa/A5htmD46xVyoxdVQfPBz7NRqYYHz9PhvKXgJtOkAY+yo0YTXDEoztQAJFY0JmXnd89tf59",
                            "notifications": [{
                                "send_date": notification.date,
                                "ignore_user_timezone": true,
                                "content": notification.message
                            }]
                        }
                    }),
                    dataType: "json"
                }).done(function(data) {
                    console.log(data);
                });
            }
        }
    }
</script>
