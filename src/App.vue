<template>
  <div id="app">
    <header class="app-header">
      <h1>
        
        DART: 
        <span style="text-decoration: underline;">D</span>ecoupled 
        <span style="text-decoration: underline;">A</span>gentic 
        <span style="text-decoration: underline;">R</span>L 
        <span style="text-decoration: underline;">T</span>raining framework for GUI agents
      </h1>
      <div class="header-nav">
        <button @click="currentView = 'overview'" :class="{ active: currentView === 'overview' }" class="nav-link">Overview</button>
        <button @click="currentView = 'trajectories'" :class="{ active: currentView === 'trajectories' }" class="nav-link">Trajectories</button>
        <button @click="currentView = 'resources'" :class="{ active: currentView === 'resources' }" class="nav-link">Code & Model</button>
      </div>
    </header>
    
    <!-- Overview Section -->
    <div v-if="currentView === 'overview'" class="overview-section">
      <div class="overview-container">
        <div class="overview-hero">
          <h1>DART</h1>
          <p class="subtitle">Efficient Multi-turn RL for GUI Agents via Decoupled Training and Adaptive Data Curation</p>
          <p class="notice-anon">🔒 Anonymous Submission for Review</p>
        </div>
        
        <div class="tldr-box">
          <div class="tldr-title">TL;DR</div>
          <p>This paper introduces DART, a decoupled RL framework for GUI agents that improves efficiency via asynchronous system design and facilitates effective learning from abundant samples through adaptive data curation.</p>
        </div>

        <!-- Resources Section on Main Page -->
        <div class="main-resources">
          <h2>📦 Resources</h2>
          <div class="resource-cards-compact">
            <div class="resource-card-compact">
              <div class="resource-header">
                <span class="resource-icon-small">💻</span>
                <h3>Source Code</h3>
              </div>
              <p class="resource-desc-small">Complete implementation including training scripts, inference code, and evaluation tools.</p>
              <div class="resource-meta">
                <span>📦 ZIP Archive (~15 MB)</span>
              </div>
              <a href="./iclr26-dart-gui.zip" download class="btn-download-compact">
                <span>⬇️</span> Download Code
              </a>
            </div>
            
            <div class="resource-card-compact">
              <div class="resource-header">
                <span class="resource-icon-small">🤖</span>
                <h3>Model Checkpoint</h3>
              </div>
              <p class="resource-desc-small">Pre-trained 7B parameter model checkpoint ready for evaluation and inference.</p>
              <div class="resource-meta">
                <span>🔢 7B Parameters (~14 GB)</span>
                <span>🤗 Hugging Face</span>
              </div>
              <a href="https://huggingface.co/dart-gui/dart-gui-7b" target="_blank" class="btn-download-compact" style="margin-bottom: 10px;">
                <span>🔗</span> View on Hugging Face(Anonymous repo)
              </a>
              <button @click="showCheckpointInfo = true" class="btn-download-compact" style="background: white; color: #667eea; border: 2px solid #667eea;">
                <span>📖</span> Usage Instructions
              </button>
            </div>
          </div>
          
          <!-- Checkpoint Info Modal -->
          <div v-if="showCheckpointInfo" class="modal-overlay" @click="showCheckpointInfo = false">
            <div class="modal-content" @click.stop>
              <div class="modal-header">
                <h3>Checkpoint Access Instructions</h3>
                <button @click="showCheckpointInfo = false" class="modal-close">×</button>
              </div>
              <div class="modal-body">
                <p class="modal-intro"><strong>Due to the large size (7B model, ~14GB), please use one of the following options:</strong></p>
                
                <div class="access-option">
                  <h4>Option 1: Hugging Face (Recommended)</h4>
                  <p>Anonymous model hosted on Hugging Face:</p>
                  <pre class="code-box">
# Model Repository: https://huggingface.co/dart-gui/dart-gui-7b

# To download:
from huggingface_hub import snapshot_download
snapshot_download(repo_id="dart-gui/dart-gui-7b")

# Or use in your code directly:
from transformers import AutoModelForCausalLM, AutoTokenizer

model = AutoModelForCausalLM.from_pretrained("dart-gui/dart-gui-7b")
tokenizer = AutoTokenizer.from_pretrained("dart-gui/dart-gui-7b")</pre>
                </div>
                
                <div class="access-option">
                  <h4>Option 2: Web Interface Download</h4>
                  <p>Download directly from Hugging Face web interface:</p>
                  <pre class="code-box">
# Visit: https://huggingface.co/dart-gui/dart-gui-7b/tree/main

# Click on individual files to download, or use "Download" button
# Files include:
# - config.json
# - pytorch_model.bin (or model shards)
# - tokenizer files
# - README.md</pre>
                </div>
                
                <div class="access-option">
                  <h4>Option 3: Using Git LFS</h4>
                  <p>Clone the repository using Git Large File Storage:</p>
                  <pre class="code-box">
# Install git-lfs first: https://git-lfs.github.com/
git lfs install

# Clone the repository
git clone https://huggingface.co/dart-gui/dart-gui-7b

# All model files will be downloaded automatically</pre>
                </div>
                
                <div class="note-box-modal">
                  <strong>Note:</strong> The model is publicly available on Hugging Face. If you encounter any issues accessing it, please check your network connection or try using a VPN if Hugging Face is blocked in your region.
                </div>
              </div>
            </div>
          </div>
        </div>
        
        <!-- Introduction -->
        <div class="intro-section">
          <h2>Introduction</h2>
          <p class="intro-text">
            Vision-language model (VLM) based GUI agents show promise for automating complex desktop and mobile tasks, 
            but face significant challenges in applying reinforcement learning (RL): 
            <strong>(1)</strong> slow multi-turn interactions with GUI environments for policy rollout, and 
            <strong>(2)</strong> insufficient high-quality agent-environment interactions for policy learning.
          </p>
          <p class="intro-text">
            To address these challenges, we propose <strong>DART</strong>, 
            a <span class="highlight-text">D</span>ecoupled <span class="highlight-text">A</span>gent <span class="highlight-text">R</span>L <span class="highlight-text">T</span>raining framework for GUI agents, 
            which coordinates heterogeneous modules in a highly decoupled manner. 
            DART separates the training system into four asynchronous modules: environment cluster, rollout service, data manager, and trainer. 
            This design enables non-blocking communication, asynchronous training, rollout-wise trajectory sampling, and per-worker model synchronization, 
            significantly improving the system efficiency: 
            <em>1.6× GPU utilization for rollout</em>, 
            <em>1.9× training throughput</em>, and 
            <em>5.5× environment utilization</em>.
          </p>
        </div>
        
        <!-- Quick Stats -->
        <div class="stats-grid">
          <div class="stat-item">
            <div class="stat-value">42.13%</div>
            <div class="stat-desc">Task Success Rate on OSWorld</div>
          </div>
          <div class="stat-item">
            <div class="stat-value">+14.61%</div>
            <div class="stat-desc">Absolute Gain over Base Model</div>
          </div>
          <div class="stat-item">
            <div class="stat-value">361</div>
            <div class="stat-desc">Example Trajectories</div>
          </div>
          <div class="stat-item">
            <div class="stat-value">7B</div>
            <div class="stat-desc">Model Parameters</div>
          </div>
        </div>
        
        <div class="quick-nav">
          <button @click="currentView = 'trajectories'" class="btn-nav-primary">
            🎯 View Trajectories
          </button>
          <button @click="currentView = 'resources'" class="btn-nav-secondary">
            📦 Download Resources
          </button>
        </div>
        
        <!-- Download All Test Trajectories Section -->
        <div class="download-trajectories-section">
          <h2>📊 Test Trajectories</h2>
          <div class="trajectory-download-card">
            <div class="trajectory-download-header">
              <span class="trajectory-icon">🗂️</span>
              <h3>All Trajectories (Tested on OSWorld)</h3>
            </div>
            <p class="trajectory-desc">Download all 361 test trajectories with step-by-step visualizations, including screenshots, actions, and rewards.</p>
            <div class="trajectory-meta">
              <span>📁 361 HTML Files</span>
              <span>🖼️ Screenshots Included</span>
              <span>📊 Action Visualizations</span>
            </div>
            <a href="https://mega.nz/folder/kmBXhCaR#l0DK4bqvwSEKlrH_WB6F6Q" target="_blank" class="btn-download-trajectories">
              <span>⬇️</span> Download All Test Trajectories
            </a>
            <p class="trajectory-note">Hosted on anonymous cloud storage for review</p>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Trajectories Section -->
    <div v-if="currentView === 'trajectories'" class="app-container">
      <!-- Left Sidebar: Trajectory List -->
      <aside class="sidebar">
        <div class="sidebar-header">
          <h2>Select Trajectory</h2>
        </div>
        
        <div v-if="loading" class="sidebar-loading">Loading...</div>
        
        <div v-else class="trajectory-list">
          <div 
            v-for="traj in trajectories" 
            :key="traj.id"
            class="trajectory-item"
            :class="{ active: selectedTrajectory && selectedTrajectory.id === traj.id }"
            @click="selectTrajectory(traj.id)"
          >
            <div class="traj-name">{{ traj.name }}</div>
            <div class="traj-info">
              <span class="badge reward" :class="getRewardClass(traj.reward)">{{ traj.reward }}</span>
              <span class="badge steps">{{ traj.steps }} steps</span>
            </div>
            <div class="traj-instruction">{{ traj.instruction }}</div>
          </div>
        </div>
      </aside>
      
      <!-- Main Content: Trajectory Visualization -->
      <main class="main-content">
        <div v-if="!selectedTrajectory" class="empty-state">
          <div class="empty-icon">📊</div>
          <h2>Select a trajectory to visualize</h2>
          <p>Choose from the list on the left to view step-by-step visualization</p>
        </div>
        
        <div v-else-if="loadingDetail" class="loading-detail">
          Loading trajectory details...
        </div>
        
        <div v-else-if="selectedTrajectory" class="trajectory-detail">
          <!-- Trajectory Info -->
          <div class="detail-header">
            <div class="detail-title">
              <h2>{{ selectedTrajectory.name }}</h2>
              <div class="detail-meta">
                <span><strong>Task ID:</strong> {{ selectedTrajectory.task_id }}</span>
                <span><strong>OS:</strong> {{ selectedTrajectory.os }}</span>
                <span><strong>Reward:</strong> <span class="badge reward" :class="getRewardClass(selectedTrajectory.reward)">{{ selectedTrajectory.reward }}</span></span>
              </div>
            </div>
            
            <div v-if="selectedTrajectory.instruction" class="instruction-box">
              <div class="instruction-label">Task Instruction:</div>
              <div class="instruction-text">{{ selectedTrajectory.instruction }}</div>
            </div>
          </div>
          
          <!-- Step Navigation -->
          <div class="step-navigation">
            <button @click="prevStep" :disabled="currentStep === 0" class="nav-btn">
              ← Previous
            </button>
            <div class="step-info">
              <span class="step-label">Step {{ currentStep + 1 }} of {{ selectedTrajectory.steps }}</span>
              <div class="step-progress">
                <div class="progress-bar" :style="{ width: ((currentStep + 1) / selectedTrajectory.steps * 100) + '%' }"></div>
              </div>
            </div>
            <button @click="nextStep" :disabled="currentStep >= selectedTrajectory.steps - 1" class="nav-btn">
              Next →
            </button>
          </div>
          
          <!-- Step Content -->
          <div v-if="currentStepData" class="step-content">
            <div v-for="(msg, idx) in currentStepData.messages" :key="idx" class="message" :class="msg.role">
              <div class="message-header">
                <span class="role-badge">{{ msg.role }}</span>
              </div>
              <div class="message-body">
                <div v-for="(content, cidx) in msg.content" :key="cidx">
                  <div v-if="content.type === 'text'" class="text-content" v-html="highlightKeywords(content.text)"></div>
                  <div v-else-if="content.type === 'image'" class="image-content">
                    <MarkedImage 
                      :imageData="content.imageData"
                      :coordinates="content.coordinates"
                      :actionType="content.actionType"
                    />
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </main>
    </div>
    
    <!-- Resources Section -->
    <div v-if="currentView === 'resources'" class="resources-section">
      <div class="resources-container">
        <div class="resources-header">
          <h1>Code & Model Resources</h1>
          <p>All resources are provided anonymously for review purposes</p>
        </div>
        
        <div class="resource-cards">
          <!-- Source Code Card -->
          <div class="resource-card">
            <div class="resource-icon">💻</div>
            <h2>Source Code</h2>
            <p class="resource-desc">Complete implementation including training scripts, inference code, and evaluation tools.</p>
            <div class="resource-details">
              <div class="detail-item">
                <span class="detail-label">Format:</span>
                <span class="detail-value">ZIP Archive</span>
              </div>
              <div class="detail-item">
                <span class="detail-label">Size:</span>
                <span class="detail-value">~15 MB</span>
              </div>
              <div class="detail-item">
                <span class="detail-label">Contents:</span>
                <span class="detail-value">Training code, inference code, configs, README</span>
              </div>
            </div>
            <div class="resource-actions">
              <a href="./iclr26-dart-gui.zip" download class="btn-download">
                <span class="btn-icon">⬇️</span>
                Download Code
              </a>
            </div>
          </div>
          
          <!-- Model Checkpoint Card -->
          <div class="resource-card">
            <div class="resource-icon">🤖</div>
            <h2>Model Checkpoint</h2>
            <p class="resource-desc">Pre-trained 7B parameter model checkpoint ready for evaluation and inference.</p>
            <div class="resource-details">
              <div class="detail-item">
                <span class="detail-label">Model Size:</span>
                <span class="detail-value">7B Parameters</span>
              </div>
              <div class="detail-item">
                <span class="detail-label">Format:</span>
                <span class="detail-value">PyTorch / Hugging Face</span>
              </div>
              <div class="detail-item">
                <span class="detail-label">Size:</span>
                <span class="detail-value">~14 GB</span>
              </div>
              <div class="detail-item">
                <span class="detail-label">Repository:</span>
                <span class="detail-value">🤗 huggingface.co/dart-gui/dart-gui-7b</span>
              </div>
            </div>
            <div class="resource-actions">
              <a href="https://huggingface.co/dart-gui/dart-gui-7b" target="_blank" class="btn-download">
                <span class="btn-icon">🔗</span>
                View on Hugging Face(Anonymous repo)
              </a>
            </div>
          </div>
          
          <!-- Complete Trajectories Dataset Card -->
          <div class="resource-card">
            <div class="resource-icon">📊</div>
            <h2>All Trajectories</h2>
            <p class="resource-desc">Full dataset with all 360+ trajectories including screenshots, actions, and annotations.</p>
            <div class="resource-details">
              <div class="detail-item">
                <span class="detail-label">Total Trajectories:</span>
                <span class="detail-value">362 trajectories</span>
              </div>
              <div class="detail-item">
                <span class="detail-label">Format:</span>
                <span class="detail-value">ZIP Archive (JSON + PNG)</span>
              </div>
              <div class="detail-item">
                <span class="detail-label">Size:</span>
                <span class="detail-value">~2.7 GB</span>
              </div>
              <div class="detail-item">
                <span class="detail-label">Contents:</span>
                <span class="detail-value">Trajectories with screenshots, actions, rewards</span>
              </div>
            </div>
            <div class="resource-actions">
              <a href="https://anonymous.4open.science/r/dart-trajectories" target="_blank" class="btn-download">
                <span class="btn-icon">🔗</span>
                Download Complete Dataset
              </a>
              <p class="resource-note">Available on Anonymous Cloud Storage</p>
            </div>
          </div>
          
        </div>
        
        <div class="anonymity-notice">
          <div class="notice-icon">🔒</div>
          <div class="notice-content">
            <h3>Anonymity Notice</h3>
            <p>All resources are provided through anonymous channels to maintain double-blind review process. 
            No identifying information is included in the code or model files. After acceptance, we will 
            release all resources with proper attribution and documentation.</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import MarkedImage from './components/MarkedImage.vue'

export default {
  name: 'App',
  components: {
    MarkedImage
  },
  data() {
    return {
      currentView: 'overview', // 'overview', 'trajectories', 'resources'
      trajectories: [],
      selectedTrajectory: null,
      loading: true,
      loadingDetail: false,
      currentStep: 0,
      showCheckpointInfo: false
    }
  },
  computed: {
    currentStepData() {
      if (!this.selectedTrajectory || !this.selectedTrajectory.stepData) return null
      return this.selectedTrajectory.stepData[this.currentStep]
    }
  },
  mounted() {
    this.loadTrajectories()
  },
  methods: {
    async loadTrajectories() {
      try {
        const baseUrl = import.meta.env.BASE_URL
        const response = await fetch(`${baseUrl}data/trajectories.json`)
        if (!response.ok) throw new Error('Failed to load trajectories')
        this.trajectories = await response.json()
        
        // Auto-select first trajectory only if in trajectories view
        if (this.trajectories.length > 0 && this.currentView === 'trajectories') {
          await this.selectTrajectory(this.trajectories[0].id)
        }
      } catch (err) {
        console.error('Error loading trajectories:', err)
      } finally {
        this.loading = false
      }
    },
    
    async selectTrajectory(id) {
      this.loadingDetail = true
      this.currentStep = 0
      
      try {
        const baseUrl = import.meta.env.BASE_URL
        
        // 从trajectories列表中获取元数据
        const trajMeta = this.trajectories.find(t => t.id === id)
        if (!trajMeta) {
          throw new Error('Trajectory not found in list')
        }
        
        // 加载final_messages.json
        const response = await fetch(`${baseUrl}data/${encodeURIComponent(id)}.json`)
        if (!response.ok) throw new Error('Failed to load trajectory')
        const messages = await response.json()
        
        // 解析消息并构建步骤数据
        const stepData = []
        let currentUserMsg = null
        
        for (let i = 0; i < messages.length; i++) {
          const msg = messages[i]
          
          if (msg.role === 'system') continue
          
          if (msg.role === 'user') {
            if (currentUserMsg) {
              // 保存上一个步骤（只有user消息，没有assistant回复）
              stepData.push({
                messages: [currentUserMsg]
              })
            }
            currentUserMsg = {
              role: 'user',
              content: []
            }
            
            // 解析user消息内容
            if (Array.isArray(msg.content)) {
              for (const item of msg.content) {
                if (item.type === 'text') {
                  currentUserMsg.content.push({
                    type: 'text',
                    text: item.text
                  })
                } else if (item.type === 'image_url') {
                  // 构建图片路径 - image_url只是文件名
                  const imagePath = `${baseUrl}data/${id}/${item.image_url}`
                  currentUserMsg.content.push({
                    type: 'image',
                    imageData: imagePath,
                    coordinates: null,
                    actionType: null
                  })
                }
              }
            }
          } else if (msg.role === 'assistant' && currentUserMsg) {
            // 创建完整的步骤
            const assistantContent = []
            
            if (Array.isArray(msg.content)) {
              for (const item of msg.content) {
                if (item.type === 'text') {
                  assistantContent.push({
                    type: 'text',
                    text: item.text
                  })
                }
              }
            }
            
            stepData.push({
              messages: [
                currentUserMsg,
                {
                  role: 'assistant',
                  content: assistantContent
                }
              ]
            })
            
            currentUserMsg = null
          }
        }
        
        // 如果还有未处理的user消息
        if (currentUserMsg) {
          stepData.push({
            messages: [currentUserMsg]
          })
        }
        
        // 构建完整的轨迹对象
        this.selectedTrajectory = {
          id: trajMeta.id,
          name: trajMeta.name,
          task_id: trajMeta.task_id,
          instruction: trajMeta.instruction,
          reward: trajMeta.reward,
          steps: stepData.length,
          os: trajMeta.os || 'Ubuntu 22.04',
          app_domain: trajMeta.app_domain,
          stepData: stepData
        }
        
      } catch (err) {
        console.error('Error loading trajectory:', err)
        alert('Failed to load trajectory: ' + err.message)
      } finally {
        this.loadingDetail = false
      }
    },
    
    prevStep() {
      if (this.currentStep > 0) {
        this.currentStep--
      }
    },
    
    nextStep() {
      if (this.currentStep < this.selectedTrajectory.steps - 1) {
        this.currentStep++
      }
    },
    
    getRewardClass(reward) {
      const r = parseFloat(reward)
      if (isNaN(r)) return 'unknown'
      if (r >= 0.8) return 'high'
      if (r >= 0.5) return 'medium'
      return 'low'
    },
    
    highlightKeywords(text) {
      if (!text) return ''
      let result = text.replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;')
      result = result.replace(/(Thought:)/g, '<span style="color: #ff6b6b; font-weight: bold;">$1</span>')
      result = result.replace(/(Action:)/g, '<span style="color: #4ecdc4; font-weight: bold;">$1</span>')
      return result
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  background: #f5f7fa;
}

#app {
  height: 100vh;
  display: flex;
  flex-direction: column;
}

.app-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 20px 30px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.app-header h1 {
  font-size: 24px;
  font-weight: 600;
}

.header-nav {
  display: flex;
  gap: 10px;
}

.nav-link {
  background: rgba(255, 255, 255, 0.1);
  color: white;
  border: 2px solid rgba(255, 255, 255, 0.3);
  padding: 8px 20px;
  border-radius: 8px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
}

.nav-link:hover {
  background: rgba(255, 255, 255, 0.2);
  border-color: rgba(255, 255, 255, 0.5);
}

.nav-link.active {
  background: white;
  color: #667eea;
  border-color: white;
}

.app-container {
  display: flex;
  flex: 1;
  overflow: hidden;
}

/* Sidebar */
.sidebar {
  width: 350px;
  background: white;
  border-right: 1px solid #e0e0e0;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.sidebar-header {
  padding: 20px;
  border-bottom: 1px solid #e0e0e0;
  background: #f8f9fa;
}

.sidebar-header h2 {
  font-size: 18px;
  color: #333;
}

.sidebar-loading {
  padding: 40px 20px;
  text-align: center;
  color: #666;
}

.trajectory-list {
  flex: 1;
  overflow-y: auto;
}

.trajectory-item {
  padding: 15px 20px;
  border-bottom: 1px solid #f0f0f0;
  cursor: pointer;
  transition: all 0.2s;
}

.trajectory-item:hover {
  background: #f8f9fa;
}

.trajectory-item.active {
  background: linear-gradient(90deg, rgba(102, 126, 234, 0.1) 0%, rgba(118, 75, 162, 0.1) 100%);
  border-left: 4px solid #667eea;
}

.traj-name {
  font-size: 13px;
  font-weight: 600;
  color: #333;
  margin-bottom: 8px;
  word-break: break-all;
}

.traj-info {
  display: flex;
  gap: 8px;
  margin-bottom: 8px;
}

.badge {
  padding: 3px 8px;
  border-radius: 10px;
  font-size: 11px;
  font-weight: 600;
}

.badge.reward.high {
  background: #d4edda;
  color: #155724;
}

.badge.reward.medium {
  background: #fff3cd;
  color: #856404;
}

.badge.reward.low {
  background: #f8d7da;
  color: #721c24;
}

.badge.reward.unknown {
  background: #e2e3e5;
  color: #383d41;
}

.badge.steps {
  background: #e3f2fd;
  color: #1565c0;
}

.traj-instruction {
  font-size: 12px;
  color: #666;
  line-height: 1.4;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/* Main Content */
.main-content {
  flex: 1;
  overflow-y: auto;
  background: #f5f7fa;
}

.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100%;
  color: #999;
}

.empty-icon {
  font-size: 64px;
  margin-bottom: 20px;
}

.empty-state h2 {
  font-size: 24px;
  margin-bottom: 10px;
  color: #666;
}

.loading-detail {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  font-size: 18px;
  color: #666;
}

.trajectory-detail {
  padding: 30px;
}

.detail-header {
  background: white;
  padding: 25px;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
  margin-bottom: 20px;
}

.detail-title h2 {
  font-size: 20px;
  color: #333;
  margin-bottom: 15px;
  word-break: break-all;
}

.detail-meta {
  display: flex;
  gap: 20px;
  font-size: 13px;
  color: #666;
  flex-wrap: wrap;
}

.instruction-box {
  margin-top: 20px;
  padding: 15px;
  background: linear-gradient(135deg, #fff3cd 0%, #ffe8a1 100%);
  border-radius: 8px;
  border-left: 4px solid #ffc107;
}

.instruction-label {
  font-size: 12px;
  font-weight: 600;
  color: #856404;
  text-transform: uppercase;
  margin-bottom: 8px;
}

.instruction-text {
  font-size: 14px;
  color: #856404;
  line-height: 1.6;
}

.step-navigation {
  background: white;
  padding: 20px 25px;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  gap: 20px;
}

.nav-btn {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 8px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s;
  white-space: nowrap;
}

.nav-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
}

.nav-btn:disabled {
  background: #ccc;
  cursor: not-allowed;
  transform: none;
}

.step-info {
  flex: 1;
}

.step-label {
  display: block;
  font-size: 16px;
  font-weight: 600;
  color: #333;
  margin-bottom: 8px;
}

.step-progress {
  height: 6px;
  background: #e0e0e0;
  border-radius: 3px;
  overflow: hidden;
}

.progress-bar {
  height: 100%;
  background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
  transition: width 0.3s ease;
}

.step-content {
  background: white;
  padding: 25px;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
}

.message {
  margin-bottom: 20px;
  padding: 20px;
  border-radius: 10px;
  border-left: 4px solid;
}

.message.system {
  background: #f8f9fa;
  border-left-color: #6c757d;
}

.message.user {
  background: #e3f2fd;
  border-left-color: #2196f3;
}

.message.assistant {
  background: #f3e5f5;
  border-left-color: #9c27b0;
}

.message-header {
  margin-bottom: 12px;
}

.role-badge {
  display: inline-block;
  padding: 4px 12px;
  border-radius: 12px;
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  background: rgba(0,0,0,0.1);
}

.message-body {
  color: #333;
}

.text-content {
  white-space: pre-wrap;
  word-wrap: break-word;
  line-height: 1.7;
  font-size: 14px;
}

.image-content {
  margin: 15px 0;
}

/* Overview Section */
.overview-section {
  flex: 1;
  overflow-y: auto;
  background: linear-gradient(120deg, #f8fbff 0%, #f3f6fa 100%);
}

.overview-container {
  max-width: 1300px;
  margin: 0 auto;
  padding: 50px 30px;
}

.overview-hero {
  text-align: center;
  margin-bottom: 40px;
}

.overview-hero h1 {
  font-size: 64px;
  color: #667eea;
  margin-bottom: 15px;
  font-weight: 800;
  letter-spacing: -1px;
}

.overview-hero .subtitle {
  font-size: 28px;
  color: #374151;
  margin-bottom: 15px;
  font-weight: 500;
  line-height: 1.3;
  max-width: 1100px;
  margin-left: auto;
  margin-right: auto;
}

.notice-anon {
  font-size: 16px;
  color: #856404;
  background: linear-gradient(90deg, #fff3cd 0%, #ffe8a1 100%);
  display: inline-block;
  padding: 8px 20px;
  border-radius: 20px;
  margin-top: 10px;
  border: 2px solid #ffc107;
  font-weight: 600;
}

.tldr-box {
  background: linear-gradient(90deg, #f3e5f5 0%, #f5fafd 100%);
  border-left: 4px solid #667eea;
  padding: 20px 30px;
  border-radius: 12px;
  margin-bottom: 50px;
  box-shadow: 0 2px 8px rgba(102, 126, 234, 0.08);
}

.tldr-title {
  font-size: 18px;
  font-weight: 700;
  color: #667eea;
  margin-bottom: 10px;
}

.tldr-box p {
  font-size: 17px;
  line-height: 1.6;
  color: #333;
  text-align: left;
}

/* Main Resources on Overview */
.main-resources {
  background: white;
  padding: 40px;
  border-radius: 16px;
  box-shadow: 0 4px 16px rgba(0,0,0,0.08);
  margin-bottom: 50px;
  border: 2px solid #e1bee7;
}

.main-resources h2 {
  font-size: 32px;
  color: #667eea;
  margin-bottom: 30px;
  text-align: center;
  font-weight: 700;
}

.resource-cards-compact {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: 25px;
}

.resource-card-compact {
  background: linear-gradient(135deg, #f8f9fa 0%, #ffffff 100%);
  padding: 30px;
  border-radius: 12px;
  border: 2px solid #e0e0e0;
  transition: all 0.3s;
}

.resource-card-compact:hover {
  border-color: #667eea;
  box-shadow: 0 6px 20px rgba(102, 126, 234, 0.15);
  transform: translateY(-3px);
}

.resource-header {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 15px;
}

.resource-icon-small {
  font-size: 36px;
}

.resource-header h3 {
  font-size: 22px;
  color: #333;
  font-weight: 700;
  margin: 0;
}

.resource-desc-small {
  font-size: 14px;
  color: #666;
  line-height: 1.5;
  margin-bottom: 15px;
}

.resource-meta {
  margin-bottom: 20px;
}

.resource-meta span {
  font-size: 13px;
  color: #667eea;
  background: #f3e5f5;
  padding: 5px 12px;
  border-radius: 12px;
  font-weight: 600;
}

.btn-download-compact {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  width: 100%;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  padding: 12px 24px;
  border-radius: 8px;
  font-size: 15px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
  text-decoration: none;
}

.btn-download-compact:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(102, 126, 234, 0.4);
}

/* Modal Styles */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.6);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 20px;
}

.modal-content {
  background: white;
  border-radius: 16px;
  max-width: 800px;
  width: 100%;
  max-height: 85vh;
  overflow-y: auto;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.3);
}

.modal-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 25px 30px;
  border-radius: 16px 16px 0 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.modal-header h3 {
  margin: 0;
  font-size: 22px;
  font-weight: 700;
}

.modal-close {
  background: rgba(255, 255, 255, 0.2);
  color: white;
  border: none;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  font-size: 28px;
  cursor: pointer;
  transition: all 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
  line-height: 1;
}

.modal-close:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: rotate(90deg);
}

.modal-body {
  padding: 30px;
}

.modal-intro {
  margin-bottom: 25px;
  font-size: 16px;
  color: #333;
}

.code-box {
  background: #2d2d2d;
  color: #f8f8f2;
  padding: 15px;
  border-radius: 8px;
  font-family: 'Monaco', 'Menlo', 'Courier New', monospace;
  font-size: 13px;
  line-height: 1.6;
  overflow-x: auto;
  margin: 0;
}

.note-box-modal {
  background: #fff3cd;
  border-left: 4px solid #ffc107;
  padding: 15px;
  border-radius: 8px;
  margin-top: 25px;
  font-size: 14px;
  color: #856404;
}

.note-box-modal strong {
  color: #856404;
}

/* Introduction Section */
.intro-section {
  background: white;
  padding: 40px;
  border-radius: 16px;
  box-shadow: 0 4px 16px rgba(0,0,0,0.08);
  margin-bottom: 50px;
  border: 2px solid #e1bee7;
}

.intro-section h2 {
  font-size: 32px;
  color: #667eea;
  margin-bottom: 25px;
  font-weight: 700;
}

.intro-text {
  font-size: 16px;
  line-height: 1.7;
  color: #333;
  margin-bottom: 20px;
  text-align: justify;
}

.highlight-text {
  text-decoration: underline;
  text-decoration-color: #667eea;
  font-weight: 600;
}

/* Stats Grid */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 20px;
  margin-bottom: 40px;
}

.stat-item {
  background: white;
  padding: 30px 20px;
  border-radius: 12px;
  text-align: center;
  border: 2px solid #e1bee7;
  box-shadow: 0 2px 8px rgba(102, 126, 234, 0.08);
  transition: all 0.3s;
}

.stat-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 20px rgba(102, 126, 234, 0.15);
  border-color: #667eea;
}

.stat-value {
  font-size: 42px;
  font-weight: 800;
  color: #667eea;
  margin-bottom: 8px;
}

.stat-desc {
  font-size: 14px;
  color: #666;
  line-height: 1.4;
}

/* Quick Nav Buttons */
.quick-nav {
  display: flex;
  justify-content: center;
  gap: 20px;
  flex-wrap: wrap;
}

.btn-nav-primary, .btn-nav-secondary {
  padding: 15px 35px;
  border-radius: 10px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  border: none;
  transition: all 0.3s;
}

.btn-nav-primary {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

.btn-nav-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(102, 126, 234, 0.4);
}

.btn-nav-secondary {
  background: white;
  color: #667eea;
  border: 2px solid #667eea;
}

.btn-nav-secondary:hover {
  background: #667eea;
  color: white;
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(102, 126, 234, 0.3);
}

/* Resources Section */
.resources-section {
  flex: 1;
  overflow-y: auto;
  background: #f5f7fa;
}

.resources-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 60px 30px;
}

.resources-header {
  text-align: center;
  margin-bottom: 50px;
}

.resources-header h1 {
  font-size: 42px;
  color: #333;
  margin-bottom: 15px;
  font-weight: 700;
}

.resources-header p {
  font-size: 18px;
  color: #666;
}

.resource-cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
  gap: 30px;
  margin-bottom: 40px;
}

.resource-card {
  background: white;
  padding: 40px;
  border-radius: 16px;
  box-shadow: 0 4px 16px rgba(0,0,0,0.1);
  transition: all 0.3s;
}

.resource-card:hover {
  box-shadow: 0 6px 24px rgba(0,0,0,0.15);
}

.resource-card.full-width {
  grid-column: 1 / -1;
}

.resource-icon {
  font-size: 56px;
  margin-bottom: 20px;
}

.resource-card h2 {
  font-size: 28px;
  color: #333;
  margin-bottom: 15px;
  font-weight: 700;
}

.resource-desc {
  font-size: 16px;
  color: #666;
  line-height: 1.6;
  margin-bottom: 25px;
}

.resource-details {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 10px;
  margin-bottom: 25px;
}

.detail-item {
  display: flex;
  justify-content: space-between;
  padding: 8px 0;
  border-bottom: 1px solid #e0e0e0;
}

.detail-item:last-child {
  border-bottom: none;
}

.detail-label {
  font-weight: 600;
  color: #333;
  font-size: 14px;
}

.detail-value {
  color: #666;
  font-size: 14px;
}

.resource-actions {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.btn-download {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  text-decoration: none;
  padding: 15px 30px;
  border-radius: 10px;
  font-size: 16px;
  font-weight: 600;
  text-align: center;
  transition: all 0.3s;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

.btn-download:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(102, 126, 234, 0.4);
}

.btn-icon {
  font-size: 20px;
}

.resource-note {
  font-size: 13px;
  color: #666;
  text-align: center;
  margin: 0;
  font-style: italic;
}

.checkpoint-info {
  margin-top: 20px;
}

.info-box {
  background: #f8f9fa;
  border: 2px solid #e0e0e0;
  border-radius: 12px;
  overflow: hidden;
}

.info-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 20px 25px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.info-header h3 {
  font-size: 18px;
  margin: 0;
}

.close-btn {
  background: rgba(255, 255, 255, 0.2);
  color: white;
  border: none;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  font-size: 24px;
  cursor: pointer;
  transition: all 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
}

.close-btn:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: rotate(90deg);
}

.info-content {
  padding: 25px;
}

.info-content > p {
  margin-bottom: 20px;
  color: #333;
}

.access-option {
  background: white;
  padding: 20px;
  border-radius: 10px;
  margin-bottom: 20px;
  border-left: 4px solid #667eea;
}

.access-option h4 {
  font-size: 16px;
  color: #667eea;
  margin-bottom: 10px;
  font-weight: 600;
}

.access-option p {
  font-size: 14px;
  color: #666;
  margin-bottom: 12px;
}

.code-block {
  display: block;
  background: #2d2d2d;
  color: #f8f8f2;
  padding: 15px;
  border-radius: 8px;
  font-family: 'Monaco', 'Menlo', 'Courier New', monospace;
  font-size: 13px;
  line-height: 1.6;
  overflow-x: auto;
}

.note-box {
  background: #fff3cd;
  border-left: 4px solid #ffc107;
  padding: 15px;
  border-radius: 8px;
  margin-top: 20px;
}

.note-box strong {
  color: #856404;
}

.guide-content {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 25px;
}

.guide-section h3 {
  font-size: 16px;
  color: #333;
  margin-bottom: 12px;
  font-weight: 600;
}

.anonymity-notice {
  background: linear-gradient(135deg, #fff3cd 0%, #ffe8a1 100%);
  border: 2px solid #ffc107;
  border-radius: 16px;
  padding: 30px;
  display: flex;
  gap: 25px;
  align-items: flex-start;
}

.notice-icon {
  font-size: 48px;
  flex-shrink: 0;
}

.notice-content h3 {
  font-size: 20px;
  color: #856404;
  margin-bottom: 10px;
  font-weight: 700;
}

.notice-content p {
  font-size: 15px;
  color: #856404;
  line-height: 1.7;
}

/* Download Trajectories Section */
.download-trajectories-section {
  margin-top: 40px;
  padding-top: 40px;
  border-top: 1px solid #e2e8f0;
}

.download-trajectories-section h2 {
  font-size: 28px;
  font-weight: 700;
  color: #2d3748;
  margin-bottom: 25px;
  text-align: center;
}

.trajectory-download-card {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 20px;
  padding: 35px;
  color: white;
  text-align: center;
  box-shadow: 0 20px 40px rgba(102, 126, 234, 0.3);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  max-width: 600px;
  margin: 0 auto;
}

.trajectory-download-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 25px 50px rgba(102, 126, 234, 0.4);
}

.trajectory-download-header {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 15px;
  margin-bottom: 20px;
}

.trajectory-icon {
  font-size: 40px;
}

.trajectory-download-header h3 {
  font-size: 24px;
  font-weight: 700;
  margin: 0;
}

.trajectory-desc {
  font-size: 16px;
  line-height: 1.6;
  margin-bottom: 25px;
  opacity: 0.95;
}

.trajectory-meta {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-bottom: 30px;
  flex-wrap: wrap;
}

.trajectory-meta span {
  background: rgba(255, 255, 255, 0.2);
  padding: 8px 16px;
  border-radius: 20px;
  font-size: 14px;
  font-weight: 500;
  backdrop-filter: blur(10px);
}

.btn-download-trajectories {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  background: white;
  color: #667eea;
  padding: 15px 30px;
  border-radius: 50px;
  text-decoration: none;
  font-weight: 700;
  font-size: 16px;
  transition: all 0.3s ease;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  margin-bottom: 15px;
}

.btn-download-trajectories:hover {
  background: #f7fafc;
  transform: translateY(-2px);
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.15);
  color: #5a67d8;
}

.trajectory-note {
  font-size: 14px;
  opacity: 0.8;
  margin: 0;
  font-style: italic;
}
</style>

