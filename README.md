addEventListener("fetch", event => {
  event.respondWith(handleRequest(event.request));
});

async function handleRequest(request) {
  const url = new URL(https://dash.cloudflare.com/c830e30ffea24c8a697cd3b0d14ef380/workers/services/view/http-proxy/production/settings);
  const target = url.searchParams.get("url");

  if (!target) {
    return new Response("Missing 'url' query parameter", {
      status: 400,
      headers: {
        "Content-Type": "text/plain",
        "Access-Control-Allow-Origin": "*"
      }
    });
  }

  try {
    const fetchResponse = await fetch(target);

    const responseHeaders = new Headers(fetchResponse.headers);
    responseHeaders.set("Access-Control-Allow-Origin", "*");
    responseHeaders.set("Access-Control-Allow-Methods", "GET, OPTIONS");
    responseHeaders.set("Access-Control-Allow-Headers", "*");

    const body = await fetchResponse.arrayBuffer();

    return new Response(body, {
      status: fetchResponse.status,
      headers: responseHeaders
    });
  } catch (error) {
    return new Response("Error fetching target URL: " + error.toString(), {
      status: 500,
      headers: {
        "Content-Type": "text/plain",
        "Access-Control-Allow-Origin": "*"
      }
    });
  }
}
