// script.js
document.addEventListener('DOMContentLoaded', function() {
    const container = document.getElementById('blog-posts-container');
    const blogURL = 'https://www.analyticdave.com/';
    const feedURL = `${blogURL}/feeds/posts/default?alt=json`;

    fetch(feedURL)
        .then(response => response.json())
        .then(data => {
            const posts = data.feed.entry;
            posts.forEach(post => {
                const title = post.title.$t;
                const link = post.link.find(l => l.rel === 'alternate').href;
                const content = post.content.$t;
                const imageSrc = post.media$thumbnail ? post.media$thumbnail.url : 'placeholder-image-url.jpg';
                
                // Create card element
                const card = document.createElement('div');
                card.className = 'card';

                // Card content
                card.innerHTML = `
                    <img src="${imageSrc}" alt="${title}">
                    <div class="card-content">
                        <h3 class="card-title">${title}</h3>
                        <p class="card-description">${content.substring(0, 100)}...</p>
                        <a class="card-link" href="${link}">Read More</a>
                    </div>
                `;

                container.appendChild(card);
            });
        })
        .catch(error => console.error('Error fetching the blog posts:', error));
});
