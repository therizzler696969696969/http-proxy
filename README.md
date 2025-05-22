# http-proxy async function handleRequest(request) {
  const { method } = request;
  const url = new URL(request.url);
  const target = url.searchParams.get("url");

  if (!target) {
    return new Response("Missing 'url' query parameter", {
      status: 400,
      headers: { "Content-Type": "text/plain" }
    });
  }

  try {
    const fetchResponse = await fetch(target, {
      method: "GET", 
      headers: {
       
      }
    });

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
      headers: { "Content-Type": "text/plain" }
    });
  }
}
