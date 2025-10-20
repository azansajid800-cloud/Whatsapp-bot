number+923185847389
const { makeWASocket, useMultiFileAuthState } = require('@whiskeysockets/baileys');

async function startBot() {
  const { state, saveCreds } = await useMultiFileAuthState('auth_info');
  const sock = makeWASocket({
    auth: state,
    printQRInTerminal: true,
  });

  sock.ev.on('creds.update', saveCreds);

  sock.ev.on('messages.upsert', async ({ messages }) => {
    const msg = messages[0];
    if (!msg.key.fromMe && msg.message?.conversation) {
      await sock.sendMessage(msg.key.remoteJid, { text: 'Hello! This is your bot.' });
    }
  });
}

startBot();
