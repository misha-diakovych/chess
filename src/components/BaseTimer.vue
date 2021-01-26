<template>
	<div class="base-timer">
		<span class="base-timer__label"
					:style="{'color': remainingPathColor}">
			{{ formattedTimeLeft }}</span>
	</div>
</template>

<script>
    const WARNING_THRESHOLD = 10;
    const ALERT_THRESHOLD = 5;

    const COLOR_CODES = {
        info: {
            color: "#000000"
        },
        warning: {
            color: "#7f7979",
            threshold: WARNING_THRESHOLD
        },
        alert: {
            color: "red",
            threshold: ALERT_THRESHOLD
        }
    };

    export default {
        props: {
            selectedTypeTime: {
                type: String,
								required: true
						}
				},
        data() {
            return {
                timePassed: 0,
                timerInterval: null
            };
        },

        computed: {
            timeLimit () {
							switch (this.selectedTypeTime) {
								case '30 minutes': return '1800';
								case '10 minutes': return '600';
								case '20 second': return '20';
								default: return '600'
              }
						},

						formattedTimeLeft() {
                const timeLeft = this.timeLeft;
                const minutes = Math.floor(timeLeft / 60);
                let seconds = timeLeft % 60;

                if (seconds < 10) {
                    seconds = `0${seconds}`;
                }

                return `${minutes}:${seconds}`;
            },

            timeLeft() {
                return this.timeLimit - this.timePassed;
            },

            remainingPathColor() {
                const { alert, warning, info } = COLOR_CODES;

                if (this.timeLeft <= alert.threshold) {
                    return alert.color;
                } else if (this.timeLeft <= warning.threshold) {
                    return warning.color;
                } else {
                    return info.color;
                }
            }
        },

        watch: {
            timeLeft(newValue) {
                if (newValue === 0) {
                    this.onTimesUp();
                }
            }
        },
				methods: {
            onTimesUp() {
                clearInterval(this.timerInterval);
            },

            startTimer() {
                this.timerInterval = setInterval(() => (this.timePassed += 1), 1000);
            }
        }
    };
</script>

<style lang="scss">
	.base-timer {
		position: relative;
		&__label {
			display: flex;
			align-items: center;
			justify-content: center;
			font-size: 34px;
		}
	}
</style>
