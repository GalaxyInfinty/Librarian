const { Client, GatewayIntentBits } = require('discord.js');
const natural = require('natural');
const express = require('express');

// Create a new Discord client
const client = new Client({
  intents: [GatewayIntentBits.Guilds, GatewayIntentBits.GuildMessages, GatewayIntentBits.MessageContent],
});

// Create a Bayesian classifier
const classifier = new natural.BayesClassifier();

// Add some example questions and answers
classifier.addDocument('who was Albert Einstein', 'Albert Einstein? Ah, just the guy who revolutionized physics. No big deal.');
classifier.addDocument('what is the theory of relativity', 'The theory of relativity? Simple, it’s how you try to understand gravity... and fail.');
classifier.addDocument('who was Isaac Newton', 'Isaac Newton? Just the guy who fell from a tree and decided to think about gravity. A genius.');
classifier.addDocument('what is quantum physics', 'Quantum physics? It’s like magic, but with numbers. Better not ask too much.');
classifier.addDocument('how does gravity work', 'Ah, gravity... always pulling you down. Literally.');

classifier.train(); // Train the classifier

// Store count of name questions
const nameQuestionCounts = {};

// List of random responses for Ayla
const aylaResponses = [
  'She is... Interesting if you know what I mean...',
  'I wish she made more mistakes... hehehe I\'m excited just to remember.',
  'Ayla... This young lady has many hidden secrets.'
];

// Function to get a random Ayla response
function getRandomAylaResponse() {
  const randomIndex = Math.floor(Math.random() * aylaResponses.length);
  return aylaResponses[randomIndex];
}

// When the bot is ready
client.once('ready', () => {
  console.log('Bot is online!');
});

// When a message is received
client.on('messageCreate', (message) => {
  // Ignore messages from the bot itself
  if (message.author.bot) return;

  // Respond to the message
  const question = message.content.toLowerCase();
  const response = classifier.getClassifications(question);

  // Logic for questions about the bot's name
  if (question.includes('what would be his name?')) {
    const userId = message.author.id;
    nameQuestionCounts[userId] = (nameQuestionCounts[userId] || 0) + 1;

    if (nameQuestionCounts[userId] === 1) {
      message.channel.send("My name... You can call me a librarian....");
    } else if (nameQuestionCounts[userId] === 3) {
      message.channel.send("If you keep asking... Maybe I should invite you to my room? hehehe");
    }

    return; // Don't process this message further
  }

  // Logic for responding when someone mentions "Librarian"
  if (question.includes('librarian')) {
    message.channel.send(`Yes ${message.author.username}, do you have any questions?`);
    return; // Don't process this message further
  }

  // Logic for "What do you think of Ayla?"
  if (question.includes('what do you think of ayla?')) {
    message.channel.send(getRandomAylaResponse());
    return; // Don't process this message further
  }

  // Logic for the novel link request
  if (question.includes('could you give me the link to the novel?')) {
    message.channel.send("... I'm not sure what the content is about... but that is one of my functions. https://www.webnovel.com/book/the-eternity-game-ayla-prision_30689824308462005");
    return; // Don't process this message further
  }

  // Logic for "Could you provide me with information?" or "Help"
  if (question.includes('could you provide me with information?') || question.includes('help')) {
    message.channel.send("Clear... Your underdeveloped mind needs information to move forward, but as I am a generous chimera, I will help you.\n\nWhat would be his name?\n\nWhat do you think of Ayla?\n\nCould you give me the Link to the Novel?\n\nCould you provide me with information?");
    return; // Don't process this message further
  }

  // Respond with classifications
  if (response.length > 0 && response[0].value > 0.5) {
    message.channel.send(`Librarian: ${response[0].label}`);
  } else {
    message.channel.send("Hmmm.... I don't see why I should answer something outside of the questions I can provide information about... Unless you're a beautiful girl, then maybe we can... Talk in my room?");
  }
});

// Keep-alive server
const app = express();
app.get('/', (req, res) => {
  res.send('Bot is alive!');
});

function keepAlive() {
  app.listen(3000, () => {
    console.log('Server is ready on port 3000.');
  });
}

keepAlive(); // Keep the bot alive
client.login(process.env.DISCORD_TOKEN); // Use a variável de ambiente para o token
