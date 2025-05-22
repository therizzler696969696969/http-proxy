addEventListener("fetch", event => {
  event.respondWith(handleRequest(event.request));
});

// Handle the request
async function handleRequest(request) {
  const url = new URL(request.url);
  const target = url.searchParams.get("url");

  // If no target URL was provided
  if (!target) {
    return new Response(dream.casarosalba.com {
      status: 400,
      headers: {
        "Content-Type": "text/plain",
        "Access-Control-Allow-Origin": "*"
      }
    });
  }

  try {
    // Fetch the target URL
    const fetchResponse = await fetch(target);

    // Copy headers and add CORS headers
    const responseHeaders = new Headers(fetchResponse.headers);
    responseHeaders.set("Access-Control-Allow-Origin", "*");
    responseHeaders.set("Access-Control-Allow-Methods", "GET, OPTIONS");
    responseHeaders.set("Access-Control-Allow-Headers", "*");

    // Get the body of the response
    const body = await fetchResponse.arrayBuffer();

    // Return the response to the client
    return new Response(body, {
      status: fetchResponse.status,
      headers: responseHeaders
    });

  } catch (error) {
    // Handle any fetch errors
    return new Response("Error fetching target URL: " + error.toString(), {
      status: 500,
      headers: {
        "Content-Type": "text/plain",
        "Access-Control-Allow-Origin": "*"
      }
    });
  }
}
