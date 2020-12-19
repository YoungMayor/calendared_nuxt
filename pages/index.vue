<template>
    <v-row justify="center" align="center">
        <v-col cols="12" sm="10" md="10">
            <v-sheet v-if="$refs.calendar">
                <h1 class="text-center">
                    {{ $refs.calendar.title }}
                </h1>
            </v-sheet>

            <v-sheet height="600" class="d-flex align-center">
                <v-btn icon class="ma-2" @click="$refs.calendar.prev()">
                    <v-icon>mdi-chevron-left</v-icon>
                </v-btn>
                <v-calendar
                    ref="calendar"
                    v-model="value"
                    :weekdays="weekday"
                    :type="type"
                    :events="events"
                    :event-overlap-mode="mode"
                    :event-overlap-threshold="30"
                    :event-color="getEventColor"
                    @change="getEvents"
                    @click:date="viewDay"
                ></v-calendar>
                <v-btn icon class="ma-2" @click="$refs.calendar.next()">
                    <v-icon>mdi-chevron-right</v-icon>
                </v-btn>
            </v-sheet>
        </v-col>

        <v-bottom-sheet v-model="sheet" persistent scrollable>
            <v-card>
                <v-card-title> Date Opened here </v-card-title>

                <v-divider></v-divider>

                <v-card-text style="height: 300px">
                    <v-data-table
                        :headers="table_headers"
                        :items="table_events"
                        :items-per-page="5"
                        :loading="table_loading"
                        :mobile-breakpoint="120"
                        class="elevation-1"
                    ></v-data-table>
                </v-card-text>

                <v-divider></v-divider>

                <v-card-actions class="text-center">
                    <v-btn color="blue darken-1" text @click="sheet = false">
                        Close
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-bottom-sheet>
    </v-row>
</template>

<script>
export default {
    head() {
        return {
            title: "Calendar Home",
        };
    },

    name: "CalendarPage",

    data: () => ({
        sheet: false,
        type: "month",
        mode: "stack",
        weekday: [0, 1, 2, 3, 4, 5, 6],
        value: "",
        events: [],
        colors: [
            "blue",
            "indigo",
            "deep-purple",
            "cyan",
            "green",
            "orange",
            "grey darken-1",
        ],
        names: [
            "Meeting",
            "Holiday",
            "PTO",
            "Travel",
            "Event",
            "Birthday",
            "Conference",
            "Party",
        ],

        table_headers: [
            { text: "Event ID", align: "start", value: "id" },
            { text: "Auction Type", value: "type" },
            { text: "Initiator", value: "initiator" },
            { text: "Product Name", value: "name" },
            { text: "Weighted Average Price (BGN/MWh)", value: "wap" },
            { text: "Load (MW)", value: "load" },
            { text: "Number of Participants", value: "participants" },
        ],
        table_events: [],
        table_loading: false,
    }),

    methods: {
        getEvents({ start, end }) {
            const events = [];

            const min = new Date(`${start.date}T00:00:00`);
            const max = new Date(`${end.date}T23:59:59`);
            const days = (max.getTime() - min.getTime()) / 86400000;
            const eventCount = this.rnd(days, days + 20);

            for (let i = 0; i < eventCount; i++) {
                const allDay = this.rnd(0, 3) === 0;
                const firstTimestamp = this.rnd(min.getTime(), max.getTime());
                const first = new Date(
                    firstTimestamp - (firstTimestamp % 900000)
                );
                const secondTimestamp = this.rnd(2, allDay ? 288 : 8) * 900000;
                const second = new Date(first.getTime() + secondTimestamp);

                events.push({
                    // name: this.names[this.rnd(0, this.names.length - 1)],
                    name: "",
                    start: first,
                    // end: second,
                    end: first,
                    // color: this.colors[this.rnd(0, this.colors.length - 1)],
                    color: "green",
                    // timed: !allDay,
                    timed: false,
                });
            }

            this.events = events;
        },

        getEventColor(event) {
            return event.color;
        },

        rnd(a, b) {
            return Math.floor((b - a + 1) * Math.random()) + a;
        },

        viewDay(payload) {
            console.log(payload);
            this.sheet = true;
            this.table_loading = true;
            this.table_events = [];

            setTimeout(() => {
                this.table_loading = false;
                this.table_events = [
                    {
                        id: "1335",
                        type: "Sale",
                        initiator: "Most Energy",
                        name:
                            "IBEX Auction (Buyers) (01.06-31.12). 2020 Baseload",
                        wap: 74.27,
                        load: "14",
                        participants: "Example User",
                    },
                    {
                        id: "1336",
                        type: "Sale",
                        initiator: "Most Energy",
                        name:
                            "IBEX Auction (Buyers) (01.06-31.12). 2020 Baseload",
                        wap: 74.27,
                        load: "14",
                        participants: "Example User",
                    },
                    {
                        id: "1337",
                        type: "Sale",
                        initiator: "Most Energy",
                        name:
                            "IBEX Auction (Buyers) (01.06-31.12). 2020 Baseload",
                        wap: 74.27,
                        load: "14",
                        participants: "Example User",
                    },
                ];
            }, 3500);
        },
    },
};
</script>
