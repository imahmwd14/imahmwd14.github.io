<html>

<head>

</head>

<body>
    <script src="https://apis.google.com/js/api.js"></script>
    <script>
        const queryString = window.location.search;
        const urlParams = new URLSearchParams(queryString);
        const API_KEY = urlParams.get('key');
        const CLEINT_ID = urlParams.get('id');

        if (API_KEY.length < 1) {
            console.error("no Key");
        }

        /**
         * Sample JavaScript code for youtube.channels.list
         * See instructions for running APIs Explorer code samples locally:
         * https://developers.google.com/explorer-help/guides/code_samples#javascript
         */

        function authenticate() {
            return gapi.auth2.getAuthInstance()
                .signIn({ scope: "https://www.googleapis.com/auth/youtube" })
                .then(function () { console.log("Sign-in successful"); },
                    function (err) { console.error("Error signing in", err); });
        }
        function loadClient() {
            gapi.client.setApiKey(API_KEY);
            return gapi.client.load("https://www.googleapis.com/discovery/v1/apis/youtube/v3/rest")
                .then(function () {
                    console.log("GAPI client loaded for API");

                    // Example 1: Use method-specific function
                    var request = gapi.client.youtube.channels.list({ 'part': 'snippet', 'mine': 'true' });

                    // Execute the API request.
                    request.execute(function (response) {
                        document.writeln(response);
                    });
                },
                    function (err) { console.error("Error loading GAPI client for API", err); });
        }
        // Make sure the client is loaded and sign-in is complete before calling this method.
        function execute() {
            return gapi.client.youtube.channels.list({
                "part": [
                    "snippet,contentDetails,statistics"
                ],
                "mine": true
            })
                .then(function (response) {
                    // Handle the results here (response.result has the parsed body).
                    console.log("Response", response);
                    document.writeln(response)
                },
                    function (err) { console.error("Execute error", err); });
        }
        gapi.load("client:auth2", function () {
            gapi.auth2.init({ client_id: CLEINT_ID });
        });
    </script>
    <button onclick="authenticate().then(loadClient)">authorize and load</button>
    <button onclick="execute()">execute</button>

</body>

</html>