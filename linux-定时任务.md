crontab -e
@reboot (sleep 60; /home/ubuntu/frp_0.52.3_linux_amd64/frps -c /home/ubuntu/frp_0.52.3_linux_amd64/frps.toml)
@reboot (sleep 30; /home/skdy/code/ssh/roadtest/server/release/server /home/skdy/code/ssh/roadtest/server/release/config.json)
