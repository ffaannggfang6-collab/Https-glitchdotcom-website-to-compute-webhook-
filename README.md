# Https-glitchdotcom-website-to-compute-webhook-
const express = require("express");
const bodyParser = require("body-parser");
const axios = require("axios");
const app = express();

app.use(bodyParser.json());

const TOKEN = "ใส่_โทเคน_LINE_ตรงนี้";

app.post("/webhook", async (req, res) => {
  const events = req.body.events || [];

  for (const event of events) {
    if (event.type === "unsend") {
      const time = new Date().toLocaleTimeString("th-TH", { hour: "2-digit", minute: "2-digit" });
      const message = {
        type: "text",
        text: `⚠️ ข้อความถูกยกเลิกเมื่อ ${time}\nข้อความ: ${event.unsend.text || "(ไม่ทราบ)"}`
      };

      await axios.post(
        "https://api.line.me/v2/bot/message/reply",
        { replyToken: event.replyToken, messages: [message] },
        { headers: { "Content-Type": "application/json", "Authorization": `Bearer ${TOKEN}` } }
      );
    }
  }

  res.sendStatus(200);
});

app.listen(process.env.PORT, () => console.log("🚀 บอร์ดทำงานแล้ว"));
