<template>
    <v-row justify="center" align="center">
        <v-col cols="12" md="10">
            <v-sheet>
                <h1 class="text-center">
                    {{ page_title }}
                </h1>
            </v-sheet>
        </v-col>

        <v-col cols="12" sm="6" lg="5">
            <v-sheet v-if="$refs.calendar_one">
                <h2 class="text-center">
                    {{ $refs.calendar_one.title }}
                </h2>
            </v-sheet>

            <v-sheet class="d-flex justify-space-between">
                <v-btn class="ma-2" @click="$refs.calendar_one.prev()">
                    <v-icon>mdi-chevron-left</v-icon>
                    Previous
                </v-btn>

                <v-btn class="ma-2" @click="$refs.calendar_one.next()">
                    Next
                    <v-icon>mdi-chevron-right</v-icon>
                </v-btn>
            </v-sheet>

            <v-sheet height="600" class="d-flex align-center">
                <v-calendar
                    ref="calendar_one"
                    v-model="calendar_one_value"
                    :weekdays="weekday"
                    :type="type"
                    :events="events"
                    :event-overlap-mode="mode"
                    :event-overlap-threshold="30"
                    :event-color="getEventColor"
                    @click:date="viewDay"
                ></v-calendar>
            </v-sheet>
        </v-col>

        <v-col cols="12" sm="6" lg="5">
            <v-sheet v-if="$refs.calendar_two">
                <h2 class="text-center">
                    {{ $refs.calendar_two.title }}
                </h2>
            </v-sheet>

            <v-sheet class="d-flex justify-space-between">
                <v-btn class="ma-2" @click="$refs.calendar_two.prev()">
                    <v-icon>mdi-chevron-left</v-icon>
                    Previous
                </v-btn>

                <v-btn class="ma-2" @click="$refs.calendar_two.next()">
                    Next
                    <v-icon>mdi-chevron-right</v-icon>
                </v-btn>
            </v-sheet>

            <v-sheet height="600" class="d-flex align-center">
                <v-calendar
                    ref="calendar_two"
                    v-model="calendar_two_value"
                    :weekdays="weekday"
                    :type="type"
                    :events="events"
                    :event-overlap-mode="mode"
                    :event-overlap-threshold="30"
                    :event-color="getEventColor"
                    @click:date="viewDay"
                ></v-calendar>
            </v-sheet>
        </v-col>

        <v-bottom-sheet v-model="sheet" persistent scrollable>
            <v-card>
                <v-card-title> {{ table_title }} </v-card-title>

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

        <v-snackbar v-model="get_event_error">
            There was an error with your request

            <template v-slot:action="{ attrs }">
                <v-btn
                    color="pink"
                    text
                    v-bind="attrs"
                    @click="get_event_error = false"
                >
                    Close
                </v-btn>
            </template>
        </v-snackbar>
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
        page_title: "",
        sheet: false,
        type: "month",
        mode: "stack",
        weekday: [0, 1, 2, 3, 4, 5, 6],
        calendar_one_value: "",
        calendar_two_value: "",
        events: [],

        table_title: "",
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
        get_event_error: false,
    }),

    methods: {
        getEventColor(event) {
            return event.color;
        },

        viewDay(payload) {
            console.log(payload);
            this.sheet = true;
            this.table_loading = true;
            this.table_events = [];
            this.table_title = "loading...";

            this.$axios
                .get(`/events/${payload.date}`)
                .then((response) => {
                    this.table_title = response.data.message;
                    this.table_events = response.data.payload;
                    console.log(response);
                })
                .catch((err) => {
                    console.log(err);
                    this.get_event_error = true;
                    this.sheet = false;
                })
                .finally(() => {
                    this.table_loading = false;
                });
        },
    },

    async asyncData(context) {
        try {
            let response = await context.$axios.get("/events");
            const title = response.data.message;
            const payload = response.data.payload;

            let events = [];
            payload.forEach((event, index) => {
                events.push({
                    name: `${event.events} event(s)`,
                    start: event.date,
                    color: "green",
                });
            });

            return {
                events: events,
                page_title: title,
            };
        } catch (err) {
            console.log(err);
        }
    },
};
</script>
